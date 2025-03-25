### TreeNode
```schema.prisma
parentId   Int?
children   TreeNode[] @relation("TreeNodeToTreeNode")
parent     TreeNode?  @relation("TreeNodeToTreeNode", fields: [parentId], references: [id])
```


