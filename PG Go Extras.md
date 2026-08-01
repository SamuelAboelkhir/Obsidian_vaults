---
tags:
- Go
- Programming-Language
MOC: Programming
---
[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Go index|Back to index]]
# Using os/exec
- Go lets us (like any other language) use commands from the OS itself
- The flow here is remarkably close to the `async/await` paradigm of [[PG TS|Typescript]]
- In the following example: 
	- A string is passed to the `echo` command
	- The `stdout` of that command is piped to the `stdin` of the following `wc` command
		- `StdoutPipe` returns an `io.ReadCloser` which is a struct including both an `io.Reader` and an `io.Closer`
		- `Stdin` is of type `io.Reader`
	- The `Start` function runs the command in as a background process on the OS itself
	- `Wait` is essential here as `Start` doesn't wait for the command to finish, and would lead to print being called on a partially filled output, leaving the main process hanging
		- Note that the process will terminate, but not cleanly
		- `Wait` listens to the exit status of the command, and releases all resources bound to the process once it's done
	- After the command finishes, `Wait` stops blocking the execution (just like `await`), and `Println` prints the final result
```go
	test := "will it output this from Go?"
	cmd := exec.Command("echo", test)
	cmd2 := exec.Command("wc", "-w")
	cmd2.Stdin, _ = cmd.StdoutPipe()
	cmd.Start()
	out, _ := cmd2.Output()
	cmd.Wait()
	fmt.Println(string(out))

```