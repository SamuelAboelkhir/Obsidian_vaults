# Big files
- Unlike small form data that we store in relational databases, big files are very large assets/files ranging from kilobytes to gigabytes
- The rule when it comes to storing data is:
	- If it makes sense to go into an excel spreadsheet, it probably belongs in a traditional database
	- If it would normally be stored on your hard drive as its own file, it probably is a "large file"
- Large files are interesting because:
	1. They're **large in size** (duh) and are thus more performance-sensitive
	2. They're usually **accessed frequently**, and this combined with their size can quickly lead to performance bottlenecks
## File attributes
- Files such as videos normally have 3 attributes to work with
	- **Metadata**: The title, description, and other information about the video
	- **Thumbnail**: An image that represents the video
	- **Video**: The actual video file
- A typicial system can allow a user to generate a draft for a video, which will include the video's metadata only
## Multipart Uploads
- So you might already be familiar with simple JSON/HTML form [POST requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST). That works great for small structured data (small strings, integers, etc.), but what about large files?
- We don't typically send massive files as single JSON payloads or forms. Instead, we use a different encoding format called [multipart/form-data](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST#multipart_form_submission). In a nutshell, it's a way to send multiple pieces of data in a single request and is commonly used for file uploads. It's the "default" way to send files to a server from an HTML form.
- The Go standard library's `net/http` package has built-in support for parsing `multipart/form-data` requests. The [http.Request](https://pkg.go.dev/net/http#Request) struct has a method called [ParseMultipartForm](https://pkg.go.dev/net/http#Request.ParseMultipartForm).
```go
func (cfg *apiConfig) handlerUploadThumbnail(w http.ResponseWriter, r *http.Request) {
    // validate the request

	const maxMemory = 10 << 20
	r.ParseMultipartForm(maxMemory)

	// "thumbnail" should match the HTML form input name
	file, header, err := r.FormFile("thumbnail")
	if err != nil {
		respondWithError(w, http.StatusBadRequest, "Unable to parse form file", err)
		return
	}
	defer file.Close()

	// `file` is an `io.Reader` that we can read from to get the image data
```
# Storage
- There are a bunch of storage solutions to consider when it comes to files
- For example, a file can be stored in memory, which may have applications for data that doesn't need to persist, but it's a fairly bad solution otherwise since memory isn't persistent
- We can also store files in a relational DB's column
	- To do so, we can actually encode the image as a [base64](https://en.wikipedia.org/wiki/Base64) string and shove the whole thing into a `text` column in SQLite. 
	- Base64 is just a way to encode binary (raw) data as text. 
	- It's not the most efficient way to do it though.
## The filesystem
- This is the first proper way to handle storage
- The problem with using base64 was:
	- **CPU performance**: Base64 encoding is an expensive CPU-intensive operation. If we have a lot of uploads (I mean, we're planning on being a _successful_ company, right?), we'll have some scaling issues.
	- **Storage costs**: Base64 encoding bloats the size of the image data. We're using more disk space than we need to, which again, is expensive and slow.
	- **Database performance**: Databases (especially relational databases like SQLite, Postgres and MySQL) are optimized for small, structured data, not giant blobs of binary. It will impact query performance in a non-trivial way.
	- **Caching**: Base64 encoded images aren't as [cache](https://en.wikipedia.org/wiki/Cache_\(computing\)) friendly as raw files, meaning slower load times and higher bandwidth costs.
- It's **usually a bad idea** to store large binary blobs in a database. There are exceptions, but they are rare. So what's the solution? **Store the files on the file system**. File systems are optimized for storing and serving files, and they do it well.
![[Pasted image 20260826112052.png|349]]
