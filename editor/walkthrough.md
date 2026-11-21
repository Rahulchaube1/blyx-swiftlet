## Distinguish 200, 404 and 405

Register a method and an absolute path template separated by one space. GET, HEAD, POST, PUT, PATCH, DELETE and OPTIONS use explicit registrations. Literal segments outrank parameters by their count. Equally specific templates use registration order. The matcher strips the query, preserves percent encoding and treats a trailing slash as significant. Hosting, URL decoding and authentication belong to the application.

1. Open an empty file named `swiftlet-demo.blyx`.
2. Type `blyx-swiftlet-demo` and select **Swiftlet — runnable example**. You can also use **Snippets: Fill File with Snippet**.
3. Choose **Blyx: Run File** from the command palette. Change an argument near the end of `fn main()` to try another input.

Expected output

```text
200
405
404
```

[Work through the example in the browser](https://www.blyx-lang.space/play?example=package-swiftlet)
