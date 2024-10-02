[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-5-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

# Quarkus - Jackson Jq

[`jq`](https://stedolan.github.io/jq/) is a very popular JSON processor.
[Jackson JQ](https://github.com/eiiches/jackson-jq) is an implementation of this processor in Java, and now you can
integrate it in your Quarkus application!

> **⚠️** Version 2.x.x of this extension (`main` branch) supports Quarkus 3, and version 1.x.x (`quarkus2` branch) supports Quarkus 2.

## Getting Started

Simply add the dependency to your Quarkus project:

```xml
<dependency>
  <groupId>io.quarkiverse.jackson-jq</groupId>
  <artifactId>quarkus-jackson-jq</artifactId>
</dependency>
```

Then you can simply use the `jackson-jq`'s
[`Scope`](https://github.com/eiiches/jackson-jq/blob/develop/1.x/jackson-jq/src/test/java/examples/Usage.java) bean in
your services. For example:

```java

@Path("/jackson-jq")
@Produces(MediaType.APPLICATION_JSON)
@Consumes(MediaType.APPLICATION_JSON)
public class JacksonJqResource {

    @Inject
    Scope scope;

    @POST
    public List<JsonNode> parse(Document document) throws JsonQueryException {
        final JsonQuery query = JsonQuery.compile(document.expression, Versions.JQ_1_6);
        List<JsonNode> out = new ArrayList<>();
        query.apply(this.scope, document.document, out::add);
        return out;
    }
}
```

## Jackson Jq Extra Module

By default, this extension already includes the [extra module](https://github.com/eiiches/jackson-jq#using-jackson-jqextras-module).

## Adding Custom functions

The extension support adding custom functions to the jq engine, as example, to add a function that accept one or two parameters:

```java
import java.util.List;

import com.fasterxml.jackson.databind.JsonNode;

import io.quarkiverse.jackson.jq.JqFunction;

import net.thisptr.jackson.jq.Expression;
import net.thisptr.jackson.jq.Function;
import net.thisptr.jackson.jq.PathOutput;
import net.thisptr.jackson.jq.Scope;
import net.thisptr.jackson.jq.Version;
import net.thisptr.jackson.jq.exception.JsonQueryException;
import net.thisptr.jackson.jq.path.Path;

@JqFunction({ "myFunction/1", "myFunction/2" })
public class MyFunction implements Function {
    @Override
    public void apply(Scope scope, List<Expression> args, JsonNode in, Path path, PathOutput output, Version version)
            throws JsonQueryException {
        
        // add the content of your function here
    }
}
```

The function will then be available to any jq expression i.e.

```shell
.items[] | myFunction("foo", "bar")
```


## Considerations

Underneath, this extension uses `jackson-jq`, so the
same [limitations and capabilities](https://github.com/eiiches/jackson-jq#implementation-status)
of that library also applies here.

If you encounter any bugs or have any questions, please feel free to open an issue.

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://ricardozanini.medium.com/"><img src="https://avatars.githubusercontent.com/u/1538000?v=4?s=100" width="100px;" alt="Ricardo Zanini"/><br /><sub><b>Ricardo Zanini</b></sub></a><br /><a href="https://github.com/quarkiverse/quarkus-jackson-jq/commits?author=ricardozanini" title="Code">💻</a> <a href="#maintenance-ricardozanini" title="Maintenance">🚧</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/eiiches"><img src="https://avatars.githubusercontent.com/u/230747?v=4?s=100" width="100px;" alt="Eiichi Sato"/><br /><sub><b>Eiichi Sato</b></sub></a><br /><a href="https://github.com/quarkiverse/quarkus-jackson-jq/pulls?q=is%3Apr+reviewed-by%3Aeiiches" title="Reviewed Pull Requests">👀</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://lburgazzoli.github.io"><img src="https://avatars.githubusercontent.com/u/1868933?v=4?s=100" width="100px;" alt="Luca Burgazzoli"/><br /><sub><b>Luca Burgazzoli</b></sub></a><br /><a href="https://github.com/quarkiverse/quarkus-jackson-jq/commits?author=lburgazzoli" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://majiehong.com"><img src="https://avatars.githubusercontent.com/u/1061229?v=4?s=100" width="100px;" alt="Jiehong"/><br /><sub><b>Jiehong</b></sub></a><br /><a href="https://github.com/quarkiverse/quarkus-jackson-jq/commits?author=Jiehong" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://thegreatapi.com"><img src="https://avatars.githubusercontent.com/u/11776454?v=4?s=100" width="100px;" alt="Helber Belmiro"/><br /><sub><b>Helber Belmiro</b></sub></a><br /><a href="https://github.com/quarkiverse/quarkus-jackson-jq/commits?author=hbelmiro" title="Code">💻</a></td>
    </tr>
  </tbody>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
