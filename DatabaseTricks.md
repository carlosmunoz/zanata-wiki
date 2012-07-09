# How to destroy your database with mysql

# Introduction

# Delete an empty project by ID

To delete an empty project 'myprojectid' by ID:
    mysql> delete HProject_Maintainer
    from HProject, HProject_Maintainer
    where HProject.slug='myprojectid' and HProject.id=HProject_Maintainer.projectId;
    Query OK, 1 row affected (0.05 sec)
    
    mysql> delete HProject
    from HProject
    where HProject.slug='myprojectid';
    Query OK, 1 row affected (0.04 sec)

# DANGER: Delete project with translations

To delete project 'TEST1' by ID (ignoring HDocumentHistory, HTextFlowHistory, HProject_Locale, HProjectIteration_Locale, HPoHeader, HPotEntryData, HSimpleComment):
    mysql> delete HTextFlowTargetHistory
    from HProject, HProjectIteration, HDocument, HTextFlow, HTextFlowTarget, HTextFlowTargetHistory
    where HProject.slug='TEST1' and HProject.id=HProjectIteration.project_id
    and HProjectIteration.id=HDocument.project_iteration_id and HDocument.id=HTextFlow.document_id
    and HTextFlow.id=HTextFlowTarget.tf_id and HTextFlowTarget.id=HTextFlowTargetHistory.target_id;
    Query OK, 10 rows affected (0.04 sec)
    
    mysql> delete HTextFlowTarget
    from HProject, HProjectIteration, HDocument, HTextFlow, HTextFlowTarget
    where HProject.slug='TEST1' and HProject.id=HProjectIteration.project_id
    and HProjectIteration.id=HDocument.project_iteration_id and HDocument.id=HTextFlow.document_id
    and HTextFlow.id=HTextFlowTarget.tf_id;
    Query OK, 5 rows affected (0.04 sec)
    
    mysql> delete HTextFlow
    from HProject, HProjectIteration, HDocument, HTextFlow
    where HProject.slug='TEST1' and HProject.id=HProjectIteration.project_id
    and HProjectIteration.id=HDocument.project_iteration_id and HDocument.id=HTextFlow.document_id;
    Query OK, 12 rows affected (0.04 sec)
    
    mysql> delete HDocument
    from HProject, HProjectIteration, HDocument
    where HProject.slug='TEST1' and HProject.id=HProjectIteration.project_id
    and HProjectIteration.id=HDocument.project_iteration_id;
    Query OK, 4 rows affected (0.06 sec)
    
    mysql> delete HProjectIteration
    from HProject, HProjectIteration
    where HProject.slug='TEST1' and HProject.id=HProjectIteration.project_id;
    Query OK, 1 row affected (0.04 sec)
    
    mysql> delete HProject_Maintainer
    from HProject, HProject_Maintainer
    where HProject.slug='TEST1' and HProject.id=HProject_Maintainer.projectId;
    Query OK, 1 row affected (0.05 sec)
    
    mysql> delete HProject
    from HProject
    where HProject.slug='TEST1';
    Query OK, 1 row affected (0.04 sec)