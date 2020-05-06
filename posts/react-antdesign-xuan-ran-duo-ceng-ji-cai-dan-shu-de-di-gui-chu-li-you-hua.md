---
title: 'React antdesign 渲染多层级菜单树的递归处理优化'
date: 2019-06-05 22:26:35
tags: [react,递归]
published: true
hideInList: false
feature: 
isTop: false
---

应用场景在于角色管理菜单树管理，这种多层`<tree>`杂糅到一起的地方，antdesign 中对`<tree>`的定义是固定的，只能渲染以这种格式：


```language-javascript
  {
      title: '0-1',
      key: '0-1',
      children: [
        { title: '0-1-0-0', key: '0-1-0-0' },
        { title: '0-1-0-1', key: '0-1-0-1' },
        { title: '0-1-0-2', key: '0-1-0-2' },
      ],
    }
```

且children的层级单一，往往后端返回的接口数据是非常多的层级，并不能立即使用。则需要对每一个层级进行单独的处理，再组合成 `<tree>` 组件所需要的格式：
```JavaScript
// 渲染树 第一级
  renderTreeNodes = data => {
    if (data) {
      const render = data.map((item, index) => {
        if (item.menuList) {
          return (
            <TreeNode title={item.moduleName} key={'0' + item.moduleId} dataRef={item}>
              {this.renderTreeNodesChildren(item.menuList)}
            </TreeNode>
          );
        }
        return <TreeNode {...item} key={'0' + item.moduleId} />;
      });
      return render;
    }
  };
  // 渲染树 第二级
  renderTreeNodesChildren = data =>
    data &&
    data.map((item, index) => {
      if (item.childrenMenuList) {
        return (
          <TreeNode title={item.menuName} key={item.menuId} dataRef={item}>
            {item.childrenMenuList.length > 0
              ? this.renderTreeNodesChildren(item.childrenMenuList)
              : this.renderTreePermissionList(item.permissionList)}
          </TreeNode>
        );
      }
      return <TreeNode {...item} key={item.menuId} />;
    });
  // 渲染树 第三级
  renderTreePermissionList = data => {
    if (data) {
      // console.log({ data });
      const render = data.map(item => {
        if (item) {
          return <TreeNode title={item.funcName} key={item.funcId} dataRef={item} />;
        }
        return <TreeNode {...item} key={item.funcId} />;
      });
      return render;
    }
  };
  ```
而对于 `<tree>`组件中 标注已选中checkbox时，需要在 componentWillUnmount() 就请求接口数据 ` this.findList(response.moduleList) ` 处理完成之后 `this.setState({
funcIdList: []})`及时更新 `<tree>`数据：
```JavaScript
 // 筛选递归
  findList = data => {
    // const { allIdList, funcIdList } = this.state;
    // console.log('findList ', data);
    for (let i = 0; i < data.length; i++) {
      const item = data[i];
      if (item.permissionList && item.permissionList.length > 0) {
        item.permissionList.map((item, index) => {
          allIdList.push(item.funcId);
          if (Number(item.grant) === 1) {
            funcIdList.push(item.funcId);
          }
        });
      } else {
        if (item.menuId) {
          allIdList.push(item.menuId);
        } else {
          allIdList.push('0' + item.moduleId);
        }
      }
      if (item.menuList && item.menuList.length > 0) {
        item.menuList.map((item, index) => {
          allIdList.push(item.menuId);
        });
        this.findList(item.menuList);
      }
      if (item.childrenMenuList && item.childrenMenuList.length > 0) {
        item.childrenMenuList.map((item, index) => {
          allIdList.push(item.menuId);
        });
        this.findList(item.childrenMenuList);
      }
    }
    const allIdDone = Array.from(new Set(allIdList));

    this.setState({
      checkedKeys: funcIdList,
      expandedKeys: allIdDone,
      allIdList: allIdDone
    });
    // console.log('allIdList', allIdDone);
    // console.log('funcIdList', funcIdList);
  };
```
