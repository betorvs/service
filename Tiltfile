custom_build(
    'ardanlabs/service/sales-api',
    'make service-push-local VERSION=$EXPECTED_TAG SERVICE_REF=$EXPECTED_REF',
    deps=['go.mod', 'go.sum', 'app/services/sales-api/', 'app/tooling/', 'business/', 'foudation/', 'vendor/', 'zarf/docker/dockerfile.service'],
    skips_local_docker=True,
)
custom_build(
    'ardanlabs/service/sales-api-metrics',
    'make metrics-push-local VERSION=$EXPECTED_TAG METRICS_REF=$EXPECTED_REF',
    deps=['go.mod', 'go.sum', 'app/services/metrics/', 'business/', 'foudation/', 'vendor/', 'zarf/docker/dockerfile.metrics'],
    skips_local_docker=True,
)
k8s_yaml(kustomize("zarf/k8s/dev/vault"), allow_duplicates=True)
k8s_yaml(kustomize("zarf/k8s/dev/database"), allow_duplicates=True)
k8s_yaml(kustomize("zarf/k8s/dev/grafana"), allow_duplicates=True)
k8s_yaml(kustomize("zarf/k8s/dev/prometheus"), allow_duplicates=True)
k8s_yaml(kustomize("zarf/k8s/dev/tempo"), allow_duplicates=True)
k8s_yaml(kustomize("zarf/k8s/dev/sales"), allow_duplicates=True)

k8s_resource('vault', port_forwards="8200:8200")
k8s_resource('database', port_forwards="5432:5432")
k8s_resource('grafana', port_forwards="3100:3100")
k8s_resource('prometheus-deployment', port_forwards="9090:9090")
k8s_resource('tempo', port_forwards="3200:3200")
k8s_resource('sales', port_forwards=["3000:3000", "4000:4000", "3001:3001"], resource_deps=['database', 'tempo', 'prometheus-deployment'])