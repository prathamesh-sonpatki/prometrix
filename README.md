Prometrix - Unified Prometheus Client
======================================================

Overview
--------

This Python package provides a unified Prometheus client that can be used to connect to and query various types of Prometheus instances. The package is based on the [prometheus-api-client](https://pypi.org/project/prometheus-api-client/)  package , which serves as the foundation for our extended functionality.

The prometrix package enhances the prometheus-api-client by adding vendor-specific authentication methods and other features to handle authorization and signatures for all supported clients. This ensures a secure and seamless connection to the various types of Prometheus instances.
1.  Coralogix
2.  GKE (Google Kubernetes Engine)
3.  Azure
4.  EKS (Amazon Elastic Kubernetes Service)
5.  Thanos
6.  Victoria Metrics

The main function, `get_custom_prometheus_connect`, allows you to create a custom Prometheus client based on the provided configuration. The configurations for special Prometheus versions are defined through specific classes, each extending the base `PrometheusConfig` class.

Installation
------------

You can install the package using pip:

```
pip install prometrix
```

Usage
-----

### Importing the package

```
from prometrix import get_custom_prometheus_connect
from prometrix.config import (
    PrometheusConfig,
    AWSPrometheusConfig,
    CoralogixPrometheusConfig,
    VictoriaMetricsPrometheusConfig,
    AzurePrometheusConfig,
    PrometheusApis,
)
```

### Creating a Custom Prometheus Client

To create a custom Prometheus client, you need to pass the appropriate configuration to the `get_custom_prometheus_connect` function. The function returns an instance of the `CustomPrometheusConnect` class that represents the client.

```
# Create a custom Prometheus client for Coralogix Prometheus
coralogix_config = CoralogixPrometheusConfig(
    url="https://coralogix-prometheus.example.com",
    prometheus_token="YOUR_CORALOGIX_PROMETHEUS_TOKEN",
    additional_labels={"job": "coralogix-prometheus"},
)
coralogix_client = get_custom_prometheus_connect(coralogix_config)

# Create a custom Prometheus client for GKE Prometheus
gke_config = PrometheusConfig(
    url="https://gke-prometheus.example.com",
    disable_ssl=False,
    headers={"Authorization": "Bearer YOUR_GKE_PROMETHEUS_TOKEN"},
    additional_labels={"job": "gke-prometheus"},
)
gke_client = get_custom_prometheus_connect(gke_config)

# Create a custom Prometheus client for Azure Prometheus
azure_config = AzurePrometheusConfig(
    url="https://azure-prometheus.example.com",
    disable_ssl=False,
    headers={"Authorization": "Bearer YOUR_AZURE_PROMETHEUS_TOKEN"},
    azure_resource="YOUR_AZURE_RESOURCE",
    azure_metadata_endpoint="https://azure-metadata.example.com",
    azure_token_endpoint="https://azure-token.example.com",
    azure_client_id="YOUR_AZURE_CLIENT_ID",
    azure_tenant_id="YOUR_AZURE_TENANT_ID",
    azure_client_secret="YOUR_AZURE_CLIENT_SECRET",
    additional_labels={"job": "azure-prometheus"},
)
azure_client = get_custom_prometheus_connect(azure_config)
```

Similar configuration and creation can be done for EKS, Thanos, and Victoria Metrics Prometheus.

> **_NOTE:_** You need to replace the placeholder values (e.g., YOUR_CORALOGIX_PROMETHEUS_TOKEN) with your actual credentials and endpoints.

### Supported APIs

The `prometrix` package extends the prometheus-api-client class PrometheusConnect with the following additional functionality:

```
get_prometheus_flags(self) -> Optional[Dict]
```
This function allows you to receive the configured flags from Prometheus. It returns a dictionary containing the flags and their respective values set in the Prometheus instance.

```
check_prometheus_connection(self, params: dict = None)
```
The `check_prometheus_connection` function enables you to check the connection status with the Prometheus instance. You can pass an optional dictionary of parameters to customize the connection check. This function returns true if it is able to connect.


Contributing
------------

If you'd like to contribute to this package, please follow the guidelines specified in the CONTRIBUTING.md file in the repository.

License
-------

This project is licensed under the MIT License - see the LICENSE.md file for details.

Acknowledgments
---------------

This package was inspired by the need to have a flexible and generic Prometheus client that can be easily extended to connect with different Prometheus types. Special thanks to the Prometheus team and the open-source community for their contributions.
