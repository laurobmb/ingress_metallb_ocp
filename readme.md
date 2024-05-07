# Configurando ingress controller com metalLB

## INstall 

  oc get clusterserviceversion -n metallb-system -o custom-columns=Name:.metadata.name,Phase:.status.phase

## Configurando ingress
Rodar esse comando para alterar o servico do ingress criado para assumir o IP do LB do METALLB

    oc patch service/router-apps-ocp-virtua -n openshift-ingress --patch '{"metadata":{"annotations":{"metallb.universe.tf/address-pool":"ingress-apps-ocp-virtua-ippool"}}}' --type=merge

Colocar essa informacao no ingress default, para que ele nao crie rotas nos seguintes namspaces

    namespaceSelector:
      matchExpressions:
      - key: kubernetes.io/metadata.name
        operator: NotIn
        values:
        - eng-residencial-dev         
        - eng-residencial-preprod     
        - eng-residencial-prod        
        - claro-3scale
        - claro-sso-prod
