<img src="assets/logo.png" width="112" height="112" alt="Swiftlet bird logo">

# Blyx Swiftlet

Dispatch a method and request path against a table of literal and parameterized routes. Return a matched template and the methods a client can use.

A method error and a missing route mean different things to an HTTP client. If their responses share one fallback, clients lose the list of methods a resource accepts. RFC 9110 requires an Allow field on a 405 response. [HTTP Semantics, §15.5.6](https://www.rfc-editor.org/rfc/rfc9110.html#name-405-method-not-allowed)

## Install and run

Install the Blyx 0.2.1 compiler and a Rust toolchain with a host linker. Clone the versioned release, then run its small example and regression suite.

```sh
cargo install --locked --version 0.2.1 blyxc
git clone --depth 1 --branch v0.1.0 https://github.com/Rahulchaube1/blyx-swiftlet.git
cd blyx-swiftlet
blyxc native-run example.blyx
blyxc native-run tests.blyx
```

Expected output from the small example

```text
200
405
404
```

## Route a book catalog request

Pass PATCH and /books/42?preview=yes to the dispatcher. It selects /books/:id and extracts 42. Try the same method on /books/new, an explicitly read-only resource. The CGI example also serves a health check through an existing web server.

```sh
blyxc native-run project.blyx
```

Compile the CGI entry point with `blyxc build cgi.blyx -o books.cgi` and install it in your server’s CGI directory. It reads REQUEST_METHOD and PATH_INFO and writes a status line, headers and a plain-text body. A HEAD request emits headers only.

Try a disallowed method with `blyxc native-run project.blyx -- DELETE /books/42`. The result includes the allowed methods.

Project output

```text
200
/books/:id
Allow: GET, PATCH
Book ID: 42
```

[Run the portable example in the browser](https://www.blyx-lang.space/play?example=package-swiftlet) · [Follow the handbook exercise](https://doc.blyx-lang.space/guide/swiftlet) · [Download the release](https://github.com/Rahulchaube1/blyx-swiftlet/releases/tag/v0.1.0)

## Use the framework in your own project

From the folder containing your entry file, clone the release into `vendor/swiftlet`.

```sh
git clone --depth 1 --branch v0.1.0 https://github.com/Rahulchaube1/blyx-swiftlet.git vendor/swiftlet
```

```blyx
use vendor::swiftlet::swiftlet;

fn main() {
    let methods = sw_enable(sw_enable(0, "GET"), "POST");
    println!(sw_status(true, methods, "GET"));
    println!(sw_status(true, methods, "DELETE"));
    println!(sw_status(false, methods, "GET"));
}
```

Run `blyxc native-run main.blyx`. For the complete native workflow also import `use vendor::swiftlet::router;`.

Register a method and an absolute path template separated by one space. GET, HEAD, POST, PUT, PATCH, DELETE and OPTIONS use explicit registrations. Literal segments outrank parameters by their count. Equally specific templates use registration order. The matcher strips the query, preserves percent encoding and treats a trailing slash as significant. Hosting, URL decoding and authentication belong to the application.

| API | Behavior |
| --- | --- |
| `sw_dispatch(routes, method, target)` | Import router. Returns status text, matched template, and the Allow value in a three-element argument list. |
| `sw_param(template, target, name)` | Extract an encoded path parameter from a matching path. An absent capture returns an empty string. |
| `sw_enable(mask, method), sw_status(found, mask, method)` | Build a method set and apply the same status decisions as the router. Start at zero. |
| `sw_allow(mask)` | Generate the comma-separated list of registered HTTP methods. |

## Contribute

When opening an issue, include the smallest reproducing Blyx input, the compiler version and the output you expected. [Source and issues](https://github.com/Rahulchaube1/blyx-swiftlet/issues) · [Blyx community](https://www.blyx-lang.space/community)

Dual-licensed under [MIT](LICENSE-MIT) or [Apache-2.0](LICENSE-APACHE).

## Connect with Rahul Chaube

[Portfolio](https://www.rahulchaube1.com/) · [GitHub](https://github.com/Rahulchaube1) · [LinkedIn](https://www.linkedin.com/in/rahulchaube1) · [Hugging Face](https://huggingface.co/oncody) · [X](https://x.com/RahulChaube_) · [EverestQ](https://www.everestqai.com/) · [Contact](https://www.rahulchaube1.com/contact)

[Blyx website](https://www.blyx-lang.space/) · [BlyxTorch](https://www.blyx-lang.space/packages/blyxtorch) · [Handbook](https://doc.blyx-lang.space/) · [Community](https://www.blyx-lang.space/community)

Support Rahul’s open-source work through [GitHub Sponsors](https://github.com/sponsors/Rahulchaube1) or [Buy Me a Coffee](https://buymeacoffee.com/rahulchaube).
