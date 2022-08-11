SOURCE_IMAGE = "tanzuapplicationplatform.azurecr.io/supply-chain/tanzu-java-web-app" #os.getenv("SOURCE_IMAGE", default='tanzuapplicationplatform.azurecr.io/supply-chain/tanzu-java-web-app')
LOCAL_PATH = "" #"C:\\Projects\\tanzu-java-web-app" # os.getenv("LOCAL_PATH", default='.')
NAMESPACE = "default" # os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'tanzu-java-web-app',
    apply_cmd="tanzu apps workload apply --live-update" +
               #" --local-path " + LOCAL_PATH +
               " --git-repo https://github.com/nycpivot/tanzu-java-web-app --git-branch main " +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes >/dev/null " +
              "&& kubectl get workload tanzu-java-web-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete tanzu-java-web-app --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    image_selector='tanzuapplicationplatform.azurecr.io/supply-chain/tanzu-java-web-app-' + NAMESPACE,
    live_update=[
      sync('target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tanzu-java-web-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'tanzu-java-web-app'}])

allow_k8s_contexts('tap-full-12')
