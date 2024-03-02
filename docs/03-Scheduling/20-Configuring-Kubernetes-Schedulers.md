# Configuring Kubernetes Schedulers
  - Take me to [video Tutorial](https://kodekloud.com/topic/configuring-kubernetes-scheduler/)

  Here's a summary of the article on "Scheduler Profiles in Kubernetes":

### Recap of Kubernetes Scheduler:
- The Kubernetes scheduler assigns pods to nodes based on resource requirements and availability.
- Pods are placed in a scheduling queue based on their priority.
- Nodes are then filtered based on their capacity to meet pod requirements.
- Next, nodes are scored, and the highest-scoring node is selected.
- Finally, the pod is bound to the selected node.

### Role of Scheduler Plugins:
- Plugins play a crucial role in each phase of pod scheduling.
- They sort pods by priority, filter out nodes without resources, and score nodes based on available resources.
- Plugins also ensure that pods are bound to nodes.

### Extension Points and Plugin Configuration:
- Kubernetes provides extension points for plugins at various stages of scheduling.
- Plugins can be customized and added to different extension points.
- Example plugins include priority sort, node resources fit, image locality, etc.

### Deployment of Separate Schedulers:
- Previously, separate scheduler binaries were used for custom scheduling.
- This approach requires managing separate processes and can lead to race conditions.

### Introduction of Scheduler Profiles:
- Kubernetes introduced scheduler profiles with the 1.18 release.
- Scheduler profiles allow multiple schedulers to run within a single binary.
- Each profile acts as a separate scheduler, enabling different scheduling behaviors.

### Configuring Scheduler Profiles:
- Scheduler profiles are configured in the scheduler's configuration file.
- Each profile can have its set of plugins enabled or disabled.
- Plugins can be specified by name or pattern for each profile.

### Benefits of Scheduler Profiles:
- Reduce the need for separate scheduler binaries.
- Prevent race conditions by enabling multiple schedulers to work within the same process.
- Enable customization of scheduling behavior for different profiles.

### Example Configuration:
- Each profile (e.g., `my-scheduler-2`, `my-scheduler-3`) can have its own set of plugins enabled or disabled.
- Plugins are configured under the `plugin` section of each profile.
- Custom plugins can be added and configured for specific profiles.

### Conclusion:
- Scheduler profiles in Kubernetes enable multiple schedulers to work within a single binary.
- They allow for better customization of scheduling behavior.
- Each profile can have its set of plugins configured to meet specific requirements.
- Scheduler profiles provide a more efficient and manageable way to handle custom scheduling needs in Kubernetes.

The article concludes by suggesting further reading on Kubernetes enhancement proposal (CAP 1451) for multi-scheduling profiles and the scheduling framework. It provides an overview of how schedulers and scheduler profiles work and how to configure them for different scheduling behaviors in Kubernetes.
_____________________________________________________________________________________________
In this section, we will take a look at configuring kubernetes schedulers.

![ks](../../images/ks.PNG)

## References
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-scheduling/scheduler.md
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
- https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/
- https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work

