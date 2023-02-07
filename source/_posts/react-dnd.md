---
categories: react
tags: 基于react-dnd实现拖拽、排序
title: 基于react-dnd实现拖拽、排序
---

# react-dnd

官网地址：<https://react-dnd.github.io/react-dnd/about>

### 1、安装

```javascript
npm install react-dnd react-dnd-html5-backend
```

### 2、 使用

##### 2.1、 DndProvider

DndProvider 组件为您的应用程序提供 React-DnD 功能。通过backend prop 注入

```javascript
import { DndProvider } from 'react-dnd';
import { HTML5Backend } from 'react-dnd-html5-backend';
import Home from '@/pages/Home';

export default function BaseLayout() {
    return (
        <DndProvider backend={ HTML5Backend }>
            <Home />
        </DndProvider>
    )
}


```

##### 2.2、 拖拽排序

官方文档中也存在示例

```javascript

  const [{ isDragging }, drag, dragPreview] = useDrag(
    () => ({
      type: ItemTypes.Box,
      item: { id, originalIndex },
      collect: (monitor) => ({
        isDragging: monitor.isDragging(),
      }),
      end: (item, monitor) => {
        const { id: droppedId, originalIndex } = item
        const didDrop = monitor.didDrop()
        // const { index: overIndex } = findCard(droppedId)
        if (!didDrop) {
          moveCard(droppedId, originalIndex)
          return
        }
      },
    }),
    [id, originalIndex, moveCard],
  )

  const [{ isOver, canDrop }, drop] = useDrop(
    () => ({
      accept: ItemTypes.Box,
      collect: (monitor) => ({
        isOver: monitor.isOver({}),
        canDrop: monitor.canDrop(),
      }),
      hover:(item: cardBase, monitor) =>{
        const { id: draggedId } = item
        if(!draggedId) {
          const hoverBoundingRect = nodeRef?.current?.getBoundingClientRect() as DOMRect;
          const hoverMiddleY = (hoverBoundingRect?.bottom - hoverBoundingRect?.top) / 2;
          const clientOffset = monitor.getClientOffset();
          if (clientOffset) {
            // Get pixels to the top
            const hoverClientY = clientOffset.y - hoverBoundingRect.top;
            if (hoverClientY <= hoverMiddleY) {
              positionRef.current = 'top'
            }
            // Dragging upwards
            if (hoverClientY > hoverMiddleY) {
              positionRef.current = 'bottom'
            }
          }
          return
        }
        if (draggedId !== id) {
          console.log(131231, )
          const { index: overIndex } = findCard(id)
          moveCard(draggedId, overIndex)
        }
        
      },
      drop:(item: cardBase, monitor)=> {
        const { id: currentId } = item
        if(currentId) return
        let { index: overIndex } = findCard(id)
        if(positionRef.current === 'bottom') overIndex += 1
        const newItem = { ...item, id: uuidv4() }
        insertCard(overIndex, newItem)
        setDragingfunc(false)
        const formData = createForm({
          effects(){
            onFormValuesChange(form=>{
              newItem.data = form.values
            })
          }
        })
        dispatch({
          type:"homeModel/setFormBox",
          payload:{
            visible:true,
            currentCard:newItem,
          },
          callback:()=>{
            toggleForm(formData)
          }
        })
        positionRef.current = null
      },
    }),
    [findCard, moveCard],
  )
```



