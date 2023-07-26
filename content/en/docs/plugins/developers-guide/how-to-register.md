---
title: How to register a plugin
linktitle: How to register a plugin
description: How to register a plugin
weight: 30
---

# Introduction

In this article, we'll focus on the steps to register and allow the community to use it.

# The registry

At the moment, what we call the `Plugin Registry` is a git repository that centralizes all available plugins through a [`yaml` file](https://github.com/falcosecurity/plugins/blob/master/registry.yaml).

The table in the [README](https://github.com/falcosecurity/plugins#registered-plugins) is auto generated by aforementioned registry:

| ID  | Name                                                                                      | Event Source     | Description                                                                                                                                                                                                                   | Info                                                                                |
| --- | ----------------------------------------------------------------------------------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| 2   | [cloudtrail](https://github.com/~~falcosecurity~~/plugins/tree/master/plugins/cloudtrail) | `aws_cloudtrail` | Reads Cloudtrail JSON logs from files/S3 and injects as events                                                                                                                                                                | Authors: [The Falco Authors](https://falco.org/community) <br/> License: Apache-2.0 |
| 3   | [dummy](https://github.com/falcosecurity/plugins/tree/master/plugins/dummy)               | `dummy`          | Reference plugin used to document interface                                                                                                                                                                                   | Authors: [The Falco Authors](https://falco.org/community) <br/> License: Apache-2.0 |
| 4   | [dummy_c](https://github.com/falcosecurity/plugins/tree/master/plugins/dummy_c)           | `dummy_c`        | Like Dummy, but written in C++                                                                                                                                                                                                | Authors: [The Falco Authors](https://falco.org/community) <br/> License: Apache-2.0 |
| 999 | test                                                                                      | `test`           | This ID is reserved for source plugin development. Any plugin author can use this ID, but authors can expect events from other developers with this ID. After development is complete, the author should request an actual ID | Authors: N/A <br/> License: N/A                                                     |

# Details of your plugin

In this section, we'll describe the key elements to get your plugin allowed to register.

The registration needs you to create a nice README for your plugin and complete all fields for the `plugins` section of [registry.yaml](https://github.com/falcosecurity/plugins/blob/master/registry.yaml), like:

```yaml
plugins:
    source:
      - id: 2
        source: aws_cloudtrail
        name: cloudtrail
        description: Reads Cloudtrail JSON logs from files/S3 and injects as events
        authors: The Falco Authors
        contact: https://falco.org/community
        url: https://github.com/falcosecurity/plugins/tree/master/plugins/cloudtrail
        license: Apache-2.0
```

## License

You're free to choose the open source license you want, you can check [https://choosealicense.com/](https://choosealicense.com/) for help. Most of the current plugins are under Apache License 2.0.

## ID

Every source plugin requires its own unique plugin event `ID` to interoperate with `Falco` and the other plugins. This `ID` is used in the following ways:

* It is stored inside in-memory event objects and used to identify the associated plugin that injected the event.
* It is stored in capture files and used to recreate in-memory event objects when reading capture files.

It must be unique to ensure that events written by a given plugin will be properly associated with that plugin (and its event sources, see below).

## Name

Each plugin in the registry must have its own `name` and can be different from `event source`, which can be shared across multiple plugins (e.g., for k8s audit logs, there might be several plugins but only one type of `event source`).

The `name` should match this regular expression `^[a-z]+[a-z0-9_]*$`.

## Fields

The `fields` are used for conditions in rules. Describe the available fields of your plugin in the README.

For example:

| Name                     | Type   | Description                    |
| ------------------------ | ------ | ------------------------------ |
| `docker.status`          | string | Status of the event            |
| `docker.id`              | string | ID of the event                |
| `docker.from`            | string | From of the event (deprecated) |
| `docker.type`            | string | Type of the event              |
| `docker.action`          | string | Action of the event            |
| `docker.stack.namespace` | string | Stack Namespace                |

# Propose your Plugin

Once you're ready, submit your plugin for registration:
* fork https://github.com/falcosecurity/plugins
* update [falcosecurity/plugins/edit/master/registry.yaml](https://github.com/falcosecurity/plugins/edit/master/registry.yaml) for adding your plugin in the `plugins` section
* submit your PR to [falcosecurity/plugins](https://github.com/falcosecurity/plugins)

> Following our [`Contributing` Guide](https://github.com/falcosecurity/.github/blob/master/CONTRIBUTING.md) your commits must be signed-off.

You can find more information [here](https://github.com/falcosecurity/plugins#registering-a-new-plugin).