---
tags:
- Other
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]
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
## Cache
- Browsers cache stuff for good reason: it makes the user experience snappier and, if the user is paying for data, _cheaper_.
- That said, sometimes (like in the last lesson) we _don't want_ the browser to cache a file - we want to be sure we have the latest version. 
### Cache busting
- One trick to ensure that we get the latest is by "busting the cache". A simple tactic is to change the URL of the file a bit. Say we have this image URL:
```
http://localhost:8080/image.jpg
```
- To cache bust, we want to alter the URL so that:
	- The **browser** thinks it's a **different** file
	- The **server** thinks it's the **same** file
- Servers typically ignore [query strings](https://en.wikipedia.org/wiki/Query_string) for file-like assets, so one of the most common ways to cache bust from the client side is to just add one. For example, a `version` parameter like this:
```
http://localhost:8080/image.jpg?version=1
```
- If we want to bust it again, we just increment the version:
```
http://localhost:8080/image.jpg?version=2
```
- _Our use of the `version` key and the `1` and `2` values are arbitrary. The important thing is that the URL is different._
### Cache Headers
- Query strings are a great way to _brute force_ cache controls as the client - but the _best_ way (assuming you have control of the server), is to use the `Cache-Control` header. Some common values are:
	- `no-store`: Don't cache this at all
	- `max-age=3600`: Cache this for 1 hour (3600 seconds)
	- `stale-while-revalidate`: Serve stale content while revalidating the cache
	- `no-cache`: Does _not_ mean "don't cache this". It means "cache this, but revalidate it before serving it again"
- _You can view [all the other options here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cache-Control)_
- When the server sends `Cache-Control` headers, it's up to the browser to respect them, but most modern browsers do.
### New Files
- "Stale" files are a common problem in web development. And when your app is small, the performance benefits of aggressively caching files might not be worth the complexity and potential bugs that can crop up from not handling cache behavior correctly. After all, the famous quote goes:
> There are only two hard things in Computer Science: **cache invalidation**, naming things, and off-by-one errors.
- That said, there is one more strategy I want to cover. It's my personal favorite for apps like Tubely.
- In Tubely, we _just don't care_ about old versions of thumbnails. Like ever. So let's just give each new thumbnail version a completely new URL (and path on the filesystem). That way, we can avoid all potential caching issues completely.
- It's not that caching is _bad_ generally (it's incredibly useful for many performance-related issues), but we know we don't need it for _this part_ of _this app_.
# Single Machine
- Let's understand why S3 being "[serverless](https://en.wikipedia.org/wiki/Serverless_computing)" is kind of a big deal.
- In a "simple" web application architecture, your server is likely a single machine running in the cloud. That single machine probably runs:
	1. An HTTP server that handles the incoming requests
	2. A database running in the background that the HTTP server talks to
	3. A file system the server uses to directly read and write larger files
	![[Pasted image 20260831231140.png|381]]
- All on one machine
## What's the big deal?
- Well, not much. Honestly this is a perfectly valid way to build a web application, even in production. That said, there are some trade-offs:
	- **Scaling**: If your app gets popular, you'll need to "scale" your single machine (add more resources like CPU/RAM/Disk space). A single computer can only become so powerful.
	- **Availability**: If your server goes down, your app goes down. To be fair, you can mitigate this with load balancers and multiple servers.
	- **Durability**: If your server crashes, or an intern `rm -rf`'s something, you're in trouble. You might have backups, but let's be honest, you probably don't.
	- **Cost**: Running a server 24/7 means _paying_ 24/7. It can be nice to only pay for what you use.
	- **Maintenance**: You have to manage everything yourself. You'll be responsible for "ops" tasks like backups, monitoring, logging, version upgrades, etc.
# AWS
- [AWS (Amazon Web Services)](https://aws.amazon.com/) is one of the (at least in my mind) "Big Three" cloud providers. The other two are [Google Cloud](https://cloud.google.com/) and [Microsoft Azure](https://azure.microsoft.com/).
- AWS is the oldest, largest, and most popular of the three, _generally speaking_.
## Serverless
- "Serverless" is an architecture (and let's be honest, a buzzword) that refers to a system where you don't have to manage the servers on your own.
- You'll often see "Serverless" used to describe services like [AWS Lambda](https://aws.amazon.com/lambda/), [Google Cloud Functions](https://cloud.google.com/functions), and [Azure Functions](https://azure.microsoft.com/en-us/services/functions/). And that's true, but it refers to "serverless" in its most "pure" form: serverless **compute**.
- [AWS S3](https://aws.amazon.com/s3/) was actually one of the first "serverless" services, and is arguably still the most popular. It's not serverless **compute**, it's serverless **storage**. You don't have to manage/scale/secure the servers that store your files, AWS does that for you.
- Instead of going to a local file system, your server makes network requests to the S3 API to read and write files
![[Pasted image 20260901005442.png]]
## Architecture
- S3 is fairly simple
- File `A` goes in bucket `B` at key `C`. That's it. You only need 2 things to access an object in S3:
	- The bucket name
	- The object key
![[Pasted image 20260902124635.png|493]]
- Buckets have globally unique names because they are part of the URL used to access them. If I make a bucket called "bd-vids", you can't make a bucket called "bd-vids", even if you're in a separate AWS account. This makes it _really easy_ to think about where your data lives.
## SDKs and S3
- An [SDK](https://en.wikipedia.org/wiki/Software_development_kit) or "Software Development Kit" is just a collection of tools (often involving an importable library) that helps you interact with a specific service or technology.
- AWS has official SDKs for most popular programming languages. They're usually the best way to interact with AWS services.
# Object Storage
- If you squint _really_ hard, it feels like S3 is a _file_ system in the cloud... but it's not. It's technically an _object_ storage system - which is _not quite_ the same thing.
## Traditional File Storage
- "File storage" is what you're already familiar with:
	- Files are stored in a hierarchy of directories
	- A file's system-level metadata (like timestamp and permissions) is managed by the _file system_, not the file itself
- File storage is great for single-machine-use (like your laptop), but it doesn't distribute well across many servers. It's optimized for low-latency access to a small number of files on a single machine.
## How Object Storage Differs
- Object storage is designed to be more **scalable, available, and durable** than file storage because it can be easily distributed across many machines:
	- Objects are stored in a flat namespace (no directories)
	- An object's metadata is stored _with_ the object itself
## File System Illusion
- Remember how I mentioned that S3's "object storage" doesn't support directories? Well, that's true, but there's some trickery involved that makes it _feel_ like it does.
- Directories are really great for organizing stuff. Storing everything in one giant bucket makes a big hard-to-manage mess. So, S3 makes your objects _feel_ like they're in directories, even though they're not.
## It's Just Prefixes
- Keys inside of a bucket are just strings. And strings can have slashes, right? **Right**.
- If you upload an object to S3 with the key `users/john/profile.jpg`, we can kind of pretend that the object is in a directory called `users` and a subdirectory called `john`. Not only that, but the S3 API actually provides tools that allow this illusion to thrive.
- Let's say I create some objects with keys:
	- `users/dan/profile.jpg`
	- `users/dan/friends.jpg`
	- `users/lane/profile.jpg`
	- `users/lane/friends.jpg`
	- `people/matt/profile.jpg`
- Then I can use the S3 API to list all the objects with the key prefix `users/lane`. It returns:
	- `users/lane/profile.jpg`
	- `users/lane/friends.jpg`
- or just everything with the prefix "users":
	- `users/dan/profile.jpg`
	- `users/dan/friends.jpg`
	- `users/lane/profile.jpg`
	- `users/lane/friends.jpg`
- It _feels_ like a hierarchy, without all the technical overhead of actually creating directories.
## Dynamic Path
- Although directories are an illusion in S3, they're still useful due to the prefix filtering capabilities of the S3 API. There are a lot of common strategies for organizing objects in S3, but the most important rule is:
> Organization matters.
- **Schema architecture matters in a SQL database, and prefix architecture matters in S3**. We always want to group objects in a way that makes sense for our case, because often we'll want to operate on a group of objects at once.
- For example, pretend you do the naive thing and upload all your images to the root of your bucket. What happens if...
	- you want to delete all the images for a specific user?
	- a feature changed and you need to resize all the images it uses?
	- you want to change the permissions of all the images associated with a specific organization?
- If you don't have any prefixes (directories) to group objects, you might find yourself iterating over every object in the bucket to find the ones you care about. That's _slow_ and _expensive_.