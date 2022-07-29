# Dapr SDK Spec - Version 1.9-alpha

## Abstract

Dapr SDK Spec is a language-neutral specification serving as a reference to Dapr SDKs.

## Table of Contents

- [Overview](#overview)
- [Notations and Terminology](#notations-and-terminology)
- [Versioning](#versioning)
- [Client Object](#client-object)
- [Object Serialization](#object-serialization)
- [Exception Handling](#exception-handling)
- [Core Building Blocks](#core-building-blocks)
- [Actors](#Actors)
- [Preview APIs](#preview-apis)

## Overview

[Dapr](https://github.com/dapr/dapr) offers a sidecar with an HTTP and gRPC API listening on localhost. In order to offer an additional layer of convenience, Dapr also offers SDKs in a variety of languages. Additionally, actors in Dapr rely on the SDK for applications to implement their actor classes and easy inocation of actor methods, timers and reminders.

Each SDK has its own set of maintainers and decisions are made per repository. Over time, decisions per SDK maintainer can deviate from a consistent experience when using Dapr in each programming language. This specification's goal is to bring standards to the Dapr experience via SDKs while still allowing each SDK to be idiomatic to its programming language and target audience.

## Notations and Terminology

### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

### Terminology

This specification defines the following terms:

#### SDK

An "SDK" or "Software Development Kit" is defined as one or more libraries that offers a set of public interfaces, methods, classes and attributes offering a programming model for Dapr native to a given programming language.

#### Domain Object

A "domain object" is defined as an instance of a class from the application's code and not part of the SDK.

## Versioning

### Spec Versioning

This spec follows [semantic versioning 2.0](https://semver.org/spec/v2.0.0.html). Matching the Dapr runtime API version for major and minor. It means that for each minor version release of Dapr runtime, there will be a new release of the spec. The patch version of the spec and runtime increase independently.

The spec will be worked on in the main branch and include the `-alpha` suffix until it reached a stable version. Once the first stable version of the spec is reached, it will be under the release-x.y branch and tagged with vX.Y.0, such as the runtime release process.

This allows SDK developers to claim compliance towards a fixed spec and runtime version instead of a moving target.

### SDK Versioning

An SDK MUST start as one repository in the [dapr-sandbox organization](https://github.com/dapr-sandbox) at version 0.1, following the process to onboard a new Dapr sandbox project. Before an SDK can graduate to the [dapr organization](https://github.com/dapr), it MUST comply to a stable version of this spec, in addition to the graduation criteria for Dapr sandbox projects.

## Client Object

The SDK MUST offer a way to instantiate a client object without requiring any parameter, using the `DAPR_HTTP_PORT` or `DAPR_GRPC_PORT` environment variables injected by the Dapr sidecar injector or Dapr CLI - defaulting to `3500` or `50001`, correspondingly, if environment variable is not set.

The client object MUST connect to the Dapr sidecar based on the `DAPR_SIDECAR_IP` environment variable, defaulting to `127.0.0.1` if not set.

The client object MUST abstract the protocol used to communicate to the Dapr sidecar and not offer gRPC or HTTP specific input or output.

The client object MIGHT offer both gRPC or HTTP implementations.

The client object SHOULD NOT implement any Actor functionality since that is handled as a separate construct.

The client object MUST implement all required methods for all [core building blocks](#core-building-blocks).

The client object MIGHT provide overloaded methods with reduced number of parameters for simpler method invocations.

The client object SHOULD NOT implement any of the methods for the [preview APIs](#alpha-apis)

## Object Serialization

The SDK MUST serialize domain objects to JSON by default.

The SDK MIGHT accept a serializer implementation via a provided interface.

The SDK MIGHT accept a separate state serializer implementation via a provided interface to differentiate serialization of transient domain objects vs persisted domain objects.

## Exception Handling

Behavior and structure for exceptions is not defined yet.

## Core Building Blocks

* [State Store](core/state-store.md)
* [PubSub](core/pubsub.md)
* [Input Binding](core/input-binding.md)
* [Output Binding](core/output-binding.md)
* [Secrets Store](core/secrets-store.md)

## Actors

An SDK MIGHT implement the Actors building block.

* [Actor client](actors/client.md)
* [Actor runtime](actors/runtime.md)

## Preview APIs

The Preview APIs MUST be offered via a preview client object implementing a separate interface from the core's client object. The preview client object MUST implement the preview APIs below:

* [Configuration](preview/configuration.md)
* [Distribured Lock](preview/distributed-lock.md)