---
layout: post
category: post
title: Angular Drag'n Drop With a Mixed Orientation (Flex Row Wrap)
date: 2019-10-26
tags: [Angular, Drag and Drop,]
comments: true
---

Angular's Drag and Drop feature supports by default [horizontal or vertical orientation](https://material.angular.io/cdk/drag-drop/overview#list-orientation). There is an open [Angular ticket #13372](https://github.com/angular/components/issues/13372) from October 2018 where you can watch Google's progress on this feature. Until then I found a simple solution :).

# tl;dr 

Solution demo on [StackBlitz](https://stackblitz.com/edit/angular-drag-n-drop-mixed-orientation-example).

The solution is:
- the template provides the div element (defining the flex row wrap) for the component,
- the component provides an items table matrix for the template to be displayed, by calculating the table matrix size based on the row's and element's width (defined e.g. in the CSS),
- each time an item is drop, the table matrix is reordered, and
- the items list input model is updated by flattening the table matrix

This follows Angular's examples and best practice where:
- a template
  - represents items - in this case a table matrix like view using flex row wrap,
  - handles drag and drop, and
  - hook a component through a drop event, and
- a component
  - holds the items input model,
  - takes care of reordering it, and
  - provides a (two dimensional) view model for the template

# Mapping a list (any[]) into a matrix (any[][]) and vice versa

One favorite solution has bein posted in [Angular ticket #13372](https://github.com/angular/components/issues/13372) and can be found on [StackBlitz](https://stackblitz.com/edit/angular-dyz1eb). It manipulates directly HTML elements manipulation by identifying pointer position.

My approach applies similar to the Angular Examples: provide a view model for the template and reorder it by using Angular's toolkit: cdkDropListGroup, moveItemInArray, and transferArrayItem. The only challenge is the view's flex row wrap dynamic behavior.

Though there are examples way of [transferring items between lists (using cdkDropListGroup)](https://material.angular.io/cdk/drag-drop/overview#transferring-items-between-lists), there is no solution of handling items in one list wrapped over multiple rows. 

But how can we map our component's item list into multiple sub-lists?

The solution for that is quite simple: the idea is mapping a one-dimensional list (represented by the input model) into a two-dimensional items table matrix (view model representing the flex row wrap).

The template looks like this:
```
<div fxLayout="row">
  <h3>Items: {{ items }}</h3>
</div>
<div #rowLayout fxLayout="row wrap" cdkDropListGroup>
  <!-- based on row layout's width and item box width, columns per row can be calculated and a items table matrix is initialized-->
  <div
    *ngFor="let itemsRow of getItemsTable(rowLayout)"
    cdkDropList
    cdkDropListOrientation="horizontal"
    [cdkDropListData]="itemsRow"
    (cdkDropListDropped)="reorderDroppedItem($event)"
  >
    <!-- drop reorders items and recalculates table matrix-->
    <div fxFlex="grow" *ngFor="let item of itemsRow" cdkDrag>
      <div class="drag-placeholder" *cdkDragPlaceholder></div>
      <div fxLayoutAlign="center center" class="item-box">{{ item }}</div>
    </div>
  </div>
</div>
```

Looking at the view where fxLayout="row wrap" is defined, basically our component's items list is wrapped into multiple rows. This can be represented as an items table matrix. All we need to know is the row's width and inside of each element's width.

# cdkDropListGroup to the rescue

Each table row can be be grouped using [https://material.angular.io/cdk/drag-drop/overview#transferring-items-between-lists](cdkDropListGroup) together. This means we group all rows together as one logical unit - for our input model.

There is only one challenge:

How can I dynamically create an items table matrix, without knowing how many items will be shown per row?

For doing that I need to know two things:

1. What is the row width?
2. What is the item (=column) width?

Based on this I can create a table matrix. In the template I have referenced my div element representing a row with #rowLayout. In the css I have defined the item-box class:

```
.item-box {
  width: 150px;
  height: 150px;
  border: solid 3px #ccc;
  background: #fff;
  font-size: 30pt;
  font-weight: bold;
  border-radius: 5px;
  margin: 0px 0px 5px 5px;
}
```

In this case my item-box width is 155 (width + margin). Passing the div element to the component, it can initialize a table matrix:

```
...
export class AppComponent {
  // width 150px + 5px margin
  boxWidth = 155;
  items: Array<number> = Array.from({ length: 20 }, (v, k) => k + 1);
  itemsTable: Array<number[]>;

  getItemsTable(rowLayout: Element): number[][] {
    if (this.itemsTable) {
      return this.itemsTable;
    }
    // calculate column size per row
    const { width } = rowLayout.getBoundingClientRect();
    const columnSize = Math.round(width / this.boxWidth);
    // calculate row size: items length / column size
    // add 0.5: round up so that last element is shown in next row
    const rowSize = Math.round(this.items.length / columnSize + .5);

    // create table rows
    const copy = [...this.items];
    this.itemsTable = Array(rowSize)
      .fill("")
      .map(
        _ =>
          Array(columnSize) // always fills to end of column size, therefore...
            .fill("")
            .map(_ => copy.shift())
            .filter(item => !!item) // ... we need to remove empty items
      );
    return this.itemsTable;
  }
...
```

Dropping an item within the same list or to another list can be done by [https://material.angular.io/cdk/drag-drop/api#functions](moveItemInArray or transferArrayItem). The second and last method takes care of dropping the dragged item:

```
  reorderDroppedItem(event: CdkDragDrop<number[]>) {
    // clone table, since it needs to be re-initialized after dropping
    let copyTableRows = this.itemsTable.map(_ => _.map(_ => _));

    // drop item
    if (event.previousContainer === event.container) {
      moveItemInArray(
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    } else {
      transferArrayItem(
        event.previousContainer.data,
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    }

    // update items after drop
    this.items = this.itemsTable.reduce((previous, current) =>
      previous.concat(current)
    );

    // re-initialize table
    let index = 0;
    this.itemsTable = copyTableRows.map(row =>
      row.map(_ => this.items[index++])
    );
  }
```

That's it!

Enjoy, Mr. T