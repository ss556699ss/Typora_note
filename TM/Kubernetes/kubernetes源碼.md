單獨啟動 scheduler

``` sh
go run scheduler.go \
	--authentication-kubeconfig=/etc/kubernetes/scheduler.conf \
	--authorization-kubeconfig=/etc/kubernetes/scheduler.conf \
	--bind-address=127.0.0.1 \ 
	--kubeconfig=/etc/kubernetes/scheduler.conf \
	--leader-elect=true

# cmd/kube-scheduler/scheduler.go 路徑下執行
sudo	go run scheduler.go \
		--kubeconfig=/etc/kubernetes/scheduler.conf 
```



scheduleOne 調度原理

``` go
func (sched *Scheduler) scheduleOne(ctx context.Context)
1. 獲取 下一個要調度的 pod
2. 獲取 pod 調度框架 ?
```

func (sched *Scheduler) scheduleOne(ctx context.Context)
1. podInfo := sched.NextPod() 獲取 下一個要調度的 pod

2. sched.frameworkForPod(pod) 獲取 pod 調度框架 ?

3. framework.NewCycleState() 新建 pod 狀態 ?

4. state.Write(framework.PodsToActivateKey, podsToActivate) 寫入 pod 狀態  ?

5. sched.schedulingCycle   schedulingCycle tries to schedule a single Pod.

   /home/roger/code/kubernetes/pkg/scheduler/schedule_one.go 

   func (sched *Scheduler) schedulingCycle

   1. sched.SchedulePod 調度節點

      /home/roger/code/kubernetes/pkg/scheduler/schedule_one.go

      func (sched *Scheduler) schedulePod

      1. sched.findNodesThatFitPod(ctx, fwk, state, pod)
      
      2.  len(pod.Status.NominatedNodeName) > 0 代表先前已經嘗試調度節點但失敗，所以k8s驅逐不合適節點，所以可能會有節點空出，所以空出的節點數大於0，這些節點優先調度
      
         sched.evaluateNominatedNode
      
         1. 
      



func (sched *Scheduler) findNodesThatPassFilters

status := fwk.RunFilterPluginsWithNominatedPods



pkg/scheduler/framework/runtime/framework.go

RunFilterPluginsWithNominatedPods



``` go
type Scheduler struct {

	SchedulePod func(ctx context.Context, fwk framework.Framework, state *framework.CycleState, pod *v1.Pod) (ScheduleResult, error)

```

