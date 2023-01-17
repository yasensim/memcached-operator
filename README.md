# memcached-operator
k8s operator to maage memcashd cluster

1. Init
`operator-sdk init --domain=yasensim.net --repo=github.com/yasensim/memcached-operator`
2. Create API
`operator-sdk create api --group cache --version v1alpha1 --kind Memcaches --plugins="deploy-image/v1-alpha" --image=memcached:1.4.36-alpine --image-container-command="memcached,-m=64,modern,-v" --run-as-user="1001"`
3. Regenerate if any changes were done
`make generate`
4. Update the CRD with the changes
`make manifest`
5. Test locally while developing
`make install run`

6. Upload the image
`docker login`
`export IMG=docker.io/yasensim/memcached-operator:v0.0.1`
`kubectl config set-context --current --namespace=memcached-operator-system`
`make docker-build IMG=$IMG`
`make docker-push IMG=$IMG`
7. Deploy the operator
`make deploy IMG=$IMG`
8. Verify deployment
`kubectl get deploy,pods`
9. Create Memcached CR and make sure it works
change cache_v1alpha1_memcaches.yaml to have 3 replicas
`kubectl apply -f config/samples/cache_v1alpha1_memcaches.yaml`
`kubectl get deploy,pods`
10. Delete the setup
`kubectl delete -f config/samples/cache_v1alpha1_memcaches.yaml`
`make undeploy`
`kubectl config set-context --current --namespace=default`


