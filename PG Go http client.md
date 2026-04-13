---
tags:
- Go
- General
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Go index|Back to index]]

# Get
## Parsing JSON into a byte slice
```Go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"io"
	"net/http"
)

func getIssueData(url string) ([]byte, error) {
	res, err := http.Get(url)
	if err != nil {
		return nil, fmt.Errorf("error creating request: %w", err)
	}
	defer res.Body.Close()

	data, err := io.ReadAll(res.Body)
	if err != nil {
		return nil, fmt.Errorf("error reading response: %w", err)
	}

	return data, nil
}

// Optional prettifying
func prettify(data string) (string, error) {
	var prettyJSON bytes.Buffer
	err := json.Indent(&prettyJSON, []byte(data), "", "  ")
	if err != nil {
		return "", fmt.Errorf("error indenting JSON: %w", err)
	}
	return prettyJSON.String(), nil
}
```
## Decoding JSON into a proper struct
- This is a cleaner approach than using `io.ReadAll()`
```Go
package main

import (
	"fmt"
	"net/http"
	"encoding/json"
)

func getIssues(url string) ([]Issue, error) {
	res, err := http.Get(url)
	if err != nil {
		return nil, fmt.Errorf("error creating request: %w", err)
	}
	defer res.Body.Close()

	var	issues []Issue
	decoder := json.NewDecoder(res.Body)

	if err := decoder.Decode(&issues); err != nil {
		fmt.Println("error decoding response body")
		return nil, err
	}
	return issues, nil
}
```
## Using Unmarshal
- While the JSON decoder `json.Decoder` streams data from an `io.Reader` into a struct, the `json.Unmarshal` doesn't need to transform the data into `[]byte` first, it skips the `io.Reader` as it works with data that's already in `[]byte`.
- `json.Decoder` is more memory efficient as it doesn't load all the data into memory at once
- `json.Unmarshal` is best with small JSON data that's already in memory
- `json.Decoder` is the standard for handling HTTP requests and responses due to the `io.Reader` step though, which allows the decoder to read from a stream of bytes, instead of loading it all first into memory as I stated previously
```Go
package main

import (
	"encoding/json"
	"fmt"
	"io"
	"net/http"
)

func getIssues(url string) ([]Issue, error) {
	res, err := http.Get(url)
	if err != nil {
		return nil, fmt.Errorf("error creating request: %w", err)
	}
	defer res.Body.Close()

	// Data must be streamed via io.ReadAll first into a []byte slice
	data, err := io.ReadAll(res.Body)
	if err != nil {
		return nil, err
	}

	var issues []Issue
	if err := json.Unmarshal(data, &issues); err != nil {
		return nil, err
	}

	return issues, nil
}
```
## JSON Marshal
- Since `json.Unmarshal` decodes `[]byte` data, `json.Marshal` does the opposite
- It converts Go structs into `[]byte` slice representations of the JSON data
```Go
package main

import (
	"encoding/json"
)

func marshalAll[T any](items []T) ([][]byte, error) {
	var marshalled [][]byte
	for _, item := range items {
		data, err := json.Marshal(item)
		if err != nil {
			return nil, err
		}
		marshalled = append(marshalled, data)
	}
	return marshalled, nil
}
```
## Unknown JSON structure
- For unknown JSON structures, we can use `map[string]interface{}` as a catchall
- Reminder that `any` in go is just an alias for `interface{}`
```Go
package main

import (
	"encoding/json"
	"fmt"
	"net/http"
	"sort"
)

func getResources(url string) ([]map[string]any, error) {
	var resources []map[string]any

	res, err := http.Get(url)
	if err != nil {
		return resources, err
	}

	defer res.Body.Close()
	var	maps []map[string]any
	
	decoder := json.NewDecoder(res.Body)

	if err := decoder.Decode(&maps); err != nil {
		fmt.Println("error decoding response body")
		return nil, err
	}
	return maps, nil

}

func logResources(resources []map[string]any) {
	var formattedStrings []string

	for _, resource := range resources {
		for key, value := range resource {
			formattedStrings = append(formattedStrings, fmt.Sprintf("Key: %s - Value: %v", key, value))
		}
	}

	sort.Strings(formattedStrings)

	for _, str := range formattedStrings {
		fmt.Println(str)
	}
}
```
## URL Sections
- Go offers a package for parsing a URL into its various sections
- A parsed URL has the following sections [[NET Components of a URL]]
```Go
return ParsedURL{
	protocol: "http",
	username: "testuser",
	password: "testpass",
	hostname: "testdomain.com",
	port:     "8080",
	pathname: "/testpath",
	search:   "testsearch=testvalue",
	hash:     "testhash",
}
```
- Put in a function
```Go
package main

import (
	"net/url"
)

func newParsedURL(urlString string) ParsedURL {
	parsedUrl, err := url.Parse(urlString)
	if err != nil {
		return ParsedURL{}
	}

	password, _ := parsedUrl.User.Password()

	return ParsedURL{
		protocol: parsedUrl.Scheme,
		username: parsedUrl.User.Username(),
		password: password,
		hostname: parsedUrl.Hostname(),
		port:     parsedUrl.Port(),
		pathname: parsedUrl.Path,
		search:   parsedUrl.RawQuery,
		hash:     parsedUrl.Fragment,
	}
}
```
### Required an optional fields in a URL

| Part     | Required                   |
| -------- | -------------------------- |
| Protocol | Yes                        |
| Username | No                         |
| Password | No                         |
| Domain   | Yes                        |
| Port     | No (defaults to 80 or 443) |
| Path     | No (defaults to /)         |
| Query    | No                         |
| Fragment | No                         |
## Headers
- To set a header
```Go
package main

import (
	"net/http"
)

func getContentType(res *http.Response) string {
	header := res.Header.Get("Content-Type")
	return header
}
```
- Keep in mind that updating the request and setting things like headers isn't dooable with methods like `http.GET`. Instead, you will need to create a `http.NewRequest`, which allows you to later edit it `req.Header.Set`
## Methods
### GET
```Go
package main

import (
	"encoding/json"
	"net/http"
)

func getUsers(url string) ([]User, error) {
	resp, err := http.Get(url)

	if err != nil {
		return nil, err
	}

	defer resp.Body.Close()
	
	decoder := json.NewDecoder(resp.Body)

	var users []User
	
	if err := decoder.Decode(&users); err != nil {
		return nil, err
	}

	return users, nil
}
```
### POST
```Go
package main

import (
	"bytes"
	"encoding/json"
	"net/http"
)

func createUser(url, apiKey string, data User) (User, error) {
	jsonData, err := json.Marshal(data)
	if err != nil {
		return User{}, err
	}

	req, err := http.NewRequest("POST", url, bytes.NewBuffer(jsonData))
	if err != nil {
		return User{}, err
	}

	req.Header.Set("Content-Type", "application/json")
	req.Header.Set("X-API-Key", apiKey)

	client := &http.Client{}
	res, err := client.Do(req)
	if err != nil {
		return User{}, err
	}
	defer res.Body.Close()

	var user User
	decoder := json.NewDecoder(res.Body)
	err = decoder.Decode(&user)
	if err != nil {
		return User{}, err
	}

	return user, nil
}
```
### PUT
- Based on Lane, `PATCH` is less popular than `PUT`, weird
```Go
package main

import (
	"bytes"
	"encoding/json"
	"net/http"
)

func updateUser(baseURL, id, apiKey string, data User) (User, error) {
	fullURL := baseURL + "/" + id

	jsonData, err := json.Marshal(data)
	if err != nil {
		return User{}, err
	}

	req, err := http.NewRequest("PUT", fullURL, bytes.NewBuffer(jsonData))

	if err != nil {
		return User{}, err
	}

	req.Header.Set("Content-Type", "application/json")
	req.Header.Set("X-API-Key", apiKey)

	res, err := http.DefaultClient.Do(req)

	if err != nil {
		return User{}, err
	}
	
	defer res.Body.Close()
	
	decoder := json.NewDecoder(res.Body)

	var user User
	
	if err := decoder.Decode(&user); err != nil {
		return User{}, err
	}

	return user, nil
}

func getUserById(baseURL, id, apiKey string) (User, error) {
	fullURL := baseURL + "/" + id

	req, err := http.NewRequest("GET", fullURL, nil)

	if err != nil {
		return User{}, err
	}
	
	req.Header.Set("X-API-Key", apiKey)

	res, err := http.DefaultClient.Do(req)
	
	if err != nil {
		return User{}, err
	}

	defer res.Body.Close()
	
	decoder := json.NewDecoder(res.Body)

	var user User
	
	if err := decoder.Decode(&user); err != nil {
		return User{}, err
	}

	return user, nil
}
```
```Go
package main

import (
	"fmt"
	"net/http"
)

func deleteUser(baseURL, id, apiKey string) error {
	fullURL := baseURL + "/" + id

	req, err := http.NewRequest("DELETE", fullURL, nil)

	if err != nil {
		fmt.Println(err)
	    return err
	}

	req.Header.Set("X-API-Key", apiKey)

	res, err := http.DefaultClient.Do(req)

	defer res.Body.Close()
	
	if res.StatusCode > 299 {
		fmt.Println(err)
		return err
	}
	
	return nil
}
```