# Test application base
Traffic -> Ingress Service -> ClusterIP Service - Deployment - 	
                              *multi-client pod *multi-client pod *multi-client pod
        				   -> ClusterIP Service - Deployment - *multi-server pod^ *multi-server pod *multi-server pod

        ^multi-server pod -> ClusterIP Service - Deployment - Postgres pod
        				  -> ClusterIP Service - Deployment - Redis pod
        				  -> Deployment - multi-worker pod
`kubectl apply -f k8s/`


Combining multiple configuration files