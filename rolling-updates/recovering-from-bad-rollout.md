## undo
1. kubectl rollout undo deploy worker
1. kubectl rollout status deploy worker

## what if undo again?
1. it will revert the bad rollout back
1. Therefore, **undo** can't be used more than once
1. How can we go back two before the bad rollout?

## Rollout history
1. You can check rollout revision number with **history**
1. kubectl rollout history deployment worker
1. if **undo** was used, some revision, some changes won't show up in the history of deployement
    - they are stored in the ReplicaSet annotations
1. kubectl describe replicasets -l app=worker | grep -A3 ^Annotations

## Rolling back to an older version
1. kubectl rollout undo deployment worker --to-revision=1
