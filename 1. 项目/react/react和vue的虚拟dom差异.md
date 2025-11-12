
### React

- ​**​虚拟 DOM 生成**：JSX 全量生成，无编译优化
- diff算法：最长递增子序列（LIS）
- 更新：状态变更即全量重渲染

### Vue

- 虚拟 DOM 生成**：模板编译 + 静态标记
- diff算法：双端算法Double-Ended Diffing
- 更新：响应式依赖追踪精准更新



两个框架的差异               Vue                React

数据流                             双                    单    
模板                                模板+指令        jsx
响应式原理                      getter/setter    setState / hooks
依赖收集                          自动                 手动 useEffect