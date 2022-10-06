
#### Is this a new Licensed Program Product (LPP)?

> It is a new member of the IBM i Portfolio of products, but it is not a traditional LPP.  Merlin is acquired through IBM Passport Advantage and the IBM Entitled Registry as a Certified Container.

#### What is Merlin?

> Merlin is a new modern IBM i development and modernization environment. It integrates  the latest development and DevOps processes  into a single product for an IBM i developer.  Merlin aligns IBM i application development with the evolving standards around  Jenkins, Git, and browser-based Theia IDE (Visual Studio Code compatible). Additionally, it integrates key modernization features such as converting fixed format RPG code into free format RPG code and application impact analyses.

#### How is Merlin priced?

> Merlin is priced per “developer”.  Because Merlin runs inside the Redhat OpenShift Container Platform (OCP), Merlin uses the built-in license monitoring tool, based on VPC (Virtual Processor Core).  Customers wishing to acquire entitlement to Merlin will order 1 VPC unit per developer, generating 1 codeready workspace for each developer. The price is $4500.00 per VPC.

#### Do I need to have OpenShift?

> Yes, Merlin, the IBM Certified Container, runs in an OpenShift environment.
>
> The OpenShift environment can be located on a Power server or anywhere OpenShift runs today.  OpenShift could also reside in a Cloud instance, for example in IBM Cloud (IBM Power Virtual Servers) or in any cloud that supports OpenShift environments. For those clients with workload running in the Cloud already, it is a natural extension to add Merlin into an OpenShift environment  also in the cloud.

#### Are there prerequisites needed for the IBM i environment? 

> IBM i needs to be at IBM i 7.3 or more current with the latest HTTP PTF Group applied. Additionally, Rational Development Studio (5770-WDS) is required for the compilers so that source code can be compiled into object code.

#### What's the Merlin IDE based on?

> The IDE is leveraging RedHat Code Ready Workspaces, incorporating  VS-Code compatible Eclipse Theia & Che for the core of the web based IDE.

#### What kind of containers are supported?  Multi-architecture? 

> Merlin is targeted for RedHat OpenShift containers running on Power or x86.

#### What about debugging capabilities?
> The debugger is a key part of a development environment and will be added to Merlin in the near term.

#### Does this product allow IBM i applications to run inside a container?

> No. IBM i Merlin is a set of tools which run in OpenShift containers. The tools guide and assist software developers in the modernization of IBM i applications and development processes, allowing them to realize the value of a hybrid cloud and multi-platform DevOps implementation.

#### Does this replace RDi?

> No, this is an alternative to using RDi for code development and modernization. Developers now have a choice of workstation-based development activities, RDi, or to use a browser, container-based option, Merlin.  Both are equally important to the IBM i development community and will continue to be enhanced and supported.
>
> However, while Merlin does have an IDE, it also provides the holistic CI/CD development ecosystem  based on Jenkins to help our IBM i development community move to an automated build and deployment  process.  

#### What is the difference between Merlin and RDi?

> Rational Developer for i (RDi) is an IDE for creating new application or updating existing native ILE applications on IBM i. Users can add plugins and additional tools to RDi to move toward a modern development ecosystem. RDi also has full support for legacy IBM i applications that are based on OPM. 
>
> Merlin is a fully integrated and supported set of tools from IBM that include an IDE and the additional plugins and tools to enable the IBM i developer to work in a modern manner. This includes Integration into a CI/CD pipeline and code modernization features like Fixed to Free conversion, native integration for Git-based source control and application impact analysis at the fingertips of every developer. Additionally, allowing integration to the automated build and deployment pipeline process.

#### Can code being updated by Merlin still be updated by RDi or SEU?

> Yes, Code updated/modified/created using Merlin's capabilities can be modified by RDi. While it is possible to also use SEU to further modify the code, as Merlin supports the latest versions of RPG and SEU does not, it is the hope that developers will have moved to a more modern coding paradigm.

#### Do customers pay extra to acquire the ARCAD functions? 

> No, those functions are integrated into the Merlin product. This means that within the IDE, developers  have full access to Fixed to Free format conversion, an integrated impact analysis  tool, and the ability to leverage intelligent  build support. These features are provided by ARCAD, but are all integrated and fully included as part of the Merlin solution.

#### Why did IBM partner with ARCAD Software?
 
> IBM and ARCAD Software have had a long-standing partnership. ARCAD had previously created plugins to the architecture being designed.  To deliver the best value to the market as fast as possible, IBM chose to work with ARCAD to deliver a product with an integrated RPG modernization and impact analysis.
