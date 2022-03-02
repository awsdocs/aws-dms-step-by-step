# Future State Architecture Design<a name="chap-oracle-postgresql.migration-process.future-state"></a>

When you migrate an Oracle application to use a different database like PostgreSQL you must capture the architecture of the existing application to ensure that all considerations are covered, we call that the current state architecture\. The current state architecture describes the part of the application that matters to the migration from an architectural point of view\. The same is true for the future state architecture which takes the new database platform into account\. We don’t need to describe everything, but some things, like external dependencies, are very important, and help us determine what work to do\.

You may already have some favorite drawing tools for architecture diagrams such as Lucidchart, Visio or the freely available [Diagrams\.net](http://diagrams.net/) which are all great choices as they supports AWS infrastructure symbols along with many others to describe the current and future environment\. But the tool is less important than what is captured in the diagrams\.

The architecture diagrams also serve the important role of defining the context of what is inside and outside the scope of work as a team collaborates on the task\.

## Current State Architecture<a name="chap-oracle-postgresql.migration-process.future-state.current-state"></a>

There may be existing documentation on the database application which should be examined for currency and relevance\. Let us review what is important for the migration work before we decide if more documentation is needed\.

A **network diagram** is useful because It typically connects servers to each other, and servers to databases\. It may also show the division into multiple availability or disaster recovery zones\. This is useful because it shows potential server and network dependencies that must be addressed in the new architecture\. A network diagram may also highlight important security considerations like multiple networks and internet connectivity\.

A **component diagram** is useful if the application is comprised of multiple parts using different technologies which each may present the migration with their own challenges to address\.

A **class diagram** is useful if it shows a specific persistence layer or a query factory where the migration can focus while leaving the rest of the application untouched\.

A **data flow diagram** is useful because it directly shows parts of the value chain of information flowing inside and outside the application highlighting what additional code may needs to be changed\.

The following image shows a simple network diagram that can help easily communicate current architecture\.

![\[A simple network diagram\]](http://docs.aws.amazon.com/dms/latest/sbs/images/oracle-postgresql-current-architecture.png)

## Future State Architecture<a name="chap-oracle-postgresql.migration-process.future-state.architecture"></a>

The future state architecture envisions the application using the new database, and potentially other services in the environment\. It’s a new version of the current state architecture diagrams with certain parts replaced with the new components\. This document will focus mainly on replacing Amazon RDS for Oracle with Amazon RDS for PostgreSQL or Aurora PostgreSQL\.

## Transition Architecture<a name="chap-oracle-postgresql.migration-process.future-state.transition"></a>

Depending on how involved your migration is, you may need a transition architecture by which we mean, infrastructure that is there only for migration purposes\. Examples of transition architecture includes AWS DMS servers and other mediating or transformation platforms\. Such infrastructure has to be provisioned, secured and removed after the migration to avoid additional vulnerability and cost\.

The following image shows a transition architecture diagram\.

![\[A transition architecture diagram\]](http://docs.aws.amazon.com/dms/latest/sbs/images/oracle-postgresql-transition-architecture.png)

For more information, see [AWS Well\-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)\.