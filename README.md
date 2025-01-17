# Tutorial Exploring the Power of Metrics Collection with OpenTelemetry on Kubernetes

This repository hosts content for Kubecon NA 2023 tutorial.

__Abstract__:
Deploying an observability system has many challenges, as several data types need to be collected. The data can be collected in many protocols and with different technology stacks. This session will cover end-to-end metrics collection on Kubernetes using the OpenTelemetry project. We will start from the ground up by instrumenting an application with OpenTelemetry APIs and agents and progressively solve more complicated use cases like a collection of resource attributes, collecting Prometheus metrics with the OpenTelemetry Collector and Operator, correlation with traces and logs, exemplars, and collecting Kubernetes infrastructure metrics.

__Schedule__:  https://kccncna2023.sched.com/event/1R2pr

__Slides__: https://docs.google.com/presentation/d/14SpOFvuXgoWh_bBeJ_Qmz5qTunD8vkxZ/edit#slide=id.p2

__Recording__:

---

## Agenda

Please set up the environment by following [the guide](./01-welcome-setup.md#deploy-initial-services) before the tutorial starts!

Each tutorial step is located in a separate file:

1. [Welcome & Setup](01-welcome-setup.md) (Pavol, 5 min)
1. [Introduction to OpenTelemetry and metrics](02-introduction-opentelemetry-metrics.md) (Pavol, Anthony)
1. [Instrumenting an app with OpenTelemetry metrics](03-app-instrumentation.md) (Bene, Pavol)
1. [Deploying collector and the app on Kubernetes](04-deploy-and-manage-collector.md) (Bene, Matej)
1. [Collecting Prometheus metrics](05-collecting-prometheus-metrics.md) (Anthony, Anusha, 15 min)
1. [Collecting Kubernetes infrastracture metrics](06-collecting-k8s-infra-metrics.md) (Matej, Anusha, 10 min)
1. [Correlation](07-correlation.md) (Pavol, 15 min)
1. Wrap up & Questions
