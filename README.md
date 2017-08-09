# Contract-SupplyChain

基于Solidity语言的，适用于供应链溯源场景的智能合约示例

# 场景描述

供应链溯源合约有以下几类参与方：

- 商品厂商：保存于`mapping(address => User) producerMap`
- 各级经销商：保存于`mapping(address => User) retailerMap`
- 消费者：保存于`mapping(address => User) customerMap`

各类参与方均通过`newUser`方法进行上链登记。通过传递不同的Actor值来指定不同参与方。

厂商首先通过`newProduct`方法将出厂商品登记到区块链，随后商品分销到下一级经销商，接下来的环节，每一次商品的分销均在接收商品的经销商处调用`retailerInnerTransfer`方法将商品进行上链登记，最终商品在零售商处由消费者购买，调用`fromRetailerToCustomer`方法进行最终登记，完成整个出厂-多级分销-零售的流程。商品一旦由厂商登记上链，便可通过`getCommodity`查询到商品当前的分销信息，只有处于该分销路径上的参与方才允许查询。此外，通过`addWhiteList`可以为指定参与方添加顶级查询权限，被添加到WhiteList的参与方，即使不参与到某商品的分销路径中，也可查询到该商品的分销信息。

