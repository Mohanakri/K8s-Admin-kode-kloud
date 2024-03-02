# Certification Tips - Imperative Commands with kubectl
  - Take me to the [Certification tips page](https://kodekloud.com/topic/certification-tips-imperative-commands-with-kubectl/)


Here's a summary of the article on Imperative and Declarative approaches in Kubernetes:

- **Objective**: The lecture discusses the Imperative and Declarative approaches in managing objects in Kubernetes, highlighting their differences and best practices.

- **Imperative Approach**:
  - In the Imperative approach, administrators give specific step-by-step instructions on what actions to take to achieve a desired state.
  - Commands like `kubectl run`, `kubectl create`, `kubectl expose`, `kubectl edit`, `kubectl scale`, `kubectl set image`, and `kubectl delete` fall under this approach.
  - Helpful for quick actions and immediate results.
  - Limited functionality for complex tasks and may require long, complex commands.
  - Commands are run once and not easily traceable or reproducible.

- **Declarative Approach**:
  - In the Declarative approach, administrators specify the desired end state of the infrastructure without detailing every step.
  - Orchestration tools like Ansible, Puppet, Chef, and Terraform fall under this category.
  - Focuses on defining the desired state in configuration files (YAML format).
  - Uses `kubectl apply` command to apply changes based on the configuration files.
  - System or software decides the steps needed to achieve the desired state.
  - Changes are easily reproducible, traceable, and can be version-controlled in Git.

- **Imperative Commands vs. Configuration Files**:
  - Imperative commands are useful for quick tasks, but limited in functionality for complex scenarios.
  - Configuration files (YAML) provide a structured way to define objects and manage complex scenarios.
  - Objects can be version-controlled, reviewed, and approved before applying changes.

- **`kubectl apply` Command**:
  - In the Declarative approach, `kubectl apply` is used to manage objects based on configuration files.
  - It creates an object if it doesn't exist and updates existing objects with changes.
  - Specifying a directory as the path allows updating multiple objects at once.

- **Managing Changes**:
  - When using `kubectl apply`, changes made to the configuration files are easily applied to the cluster.
  - The command determines the necessary changes to bring objects to the desired state.
  - It avoids errors related to object existence or updates.
  
- **Exam Tips**:
  - Use Imperative commands for quick tasks to save time in exams.
  - `kubectl edit` is helpful for editing properties of existing objects.
  - For complex scenarios, use configuration files (`kubectl create -f`) and `kubectl apply`.
  - Practicing both Imperative and Declarative approaches is essential for exam readiness.

- **Recommendations**:
  - Administrators are encouraged to practice both Imperative and Declarative approaches.
  - Understanding when to use each approach based on the task complexity is crucial.
  - Referencing the Kubernetes documentation for detailed information on managing a Kubernetes cluster.

In conclusion, the Imperative approach involves giving direct commands to Kubernetes, useful for quick tasks but limited in handling complex scenarios. On the other hand, the Declarative approach focuses on defining the desired state in configuration files, allowing for reproducibility, traceability, and version control. Admins should practice both approaches and choose the one suitable for the task at hand.
