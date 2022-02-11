# Dapr Pub & Sub Java Example

Based on https://docs.dapr.io/developing-applications/building-blocks/pubsub/howto-publish-subscribe/

UPDATE: This now preserves the **not** working state to aid this issue: https://github.com/dapr/docs/issues/2193

## Goal

Have two services - 'checkout' and 'orderprocessing' - subscribe and publish to the message broker, respectively.

## Run

Have Dapr up and running like explained in the docs.

In one terminal:

```bash
cd demo-checkout
dapr run --app-id checkout --app-port 6002 --dapr-http-port 3602 --dapr-grpc-port 60002 --components-path ../my-components mvn spring-boot:run
```

In another terminal:

```bash
cd demo-order
dapr run --app-id orderprocessing --app-port 6001 --dapr-http-port 3601 --dapr-grpc-port 60001 --components-path ../my-components mvn spring-boot:run
```

## Problem

Both apps are running and are able to do what they should, each individually, in isolation. But apparently the `checkout` app is not hooked up to the message stream as subscriber. Or in other words, its `getCheckout` method is never invoked, neither with the correct input published by the orderprocessing app (as it should) nor in any other capacity.

Zipkin shows published events by `orderprocessing`.
