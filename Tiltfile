SOURCE_IMAGE = os.getenv("SOURCE_IMAGE")
LOCAL_PATH = os.getenv("LOCAL_PATH")
NAMESPACE = os.getenv("NAMESPACE")

k8s_custom_deploy(
    'spring-sensors',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null" +
               " && kubectl get workload spring-sensors --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f tap/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update = [
        sync('./target/classes', '/workspace/BOOT-INF/classes')
      ]
)

k8s_resource('spring-sensors', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'spring-sensors'}])
allow_k8s_contexts('gke_crossplane-playground_us-west1-a_garreeoke1')