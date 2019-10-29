---
layout: post
category: post
title: Angular Drag'n Drop With a Mixed Orientation (Flex Row Wrap)
date: 2019-10-26
tags: [Angular, Drag and Drop,]
comments: true
---

Angular's Drag and Drop feature supports either [horizontal or vertical orientation](https://material.angular.io/cdk/drag-drop/overview#list-orientation) - but not both. There is an open [Angular ticket #13372](https://github.com/angular/components/issues/13372) from October 2018 where you can watch Google's progress on this feature request. Until then I found a simple solution :).

# tl;dr 

A demo can be found on [StackBlitz](https://stackblitz.com/edit/angular-drag-n-drop-mixed-orientation-example).

My solution my template and component do this:
- **the template displays a table (list of lists ) as view model**
- **the component transforms an input model (items list) into a view model (list of lists)**
- **the template dragging and dropping happens on a view model**
- **the component drop items within a view model**
- **the component transforms a view model back to its input model**

# Best Practices by Examples

This follows Angular's examples and best practice where:
- the template
  - represents items - in this case a table matrix-like view using flex row wrap,
  - uses drag and drop directives, and
  - provides event hooks calling component functions, and
- the component
  - holds the items input model,
  - takes care of reordering it, and
  - provides a (two dimensional) view model for the template.

One favorite solution that has been posted in Angular ticket #13372 can be found on [StackBlitz](https://stackblitz.com/edit/angular-dyz1eb). On dragging and dropping it reorders directly HTML elements.

Unlike that - this approach applies similar to the Angular Examples: provide a view model for the template and reorder it by using Angular's cdkDropListGroup directive and CDK's moveItemInArray and transferArrayItem functions.

# Solution - Step by Step

Basically the solution covers these 3 main parts:

## Template

Define a template with this structure:

```
- 'table' div: top element for table
  - 'row' divs: child elements for rows
    - 'column' divs: another sub-element for items
```

## CSS Style

Here we just a style class called 'item-box' with a fix width. For better visibility a placeholder class is defined while an item is dragged.

## Component

The component covers some properties and three main functions:
```
  // one dimensional input model
  items: Array<number> = Array.from({ length: 21 }, (v, k) => k + 1);
  // two dimensional table matrix representing view model
  itemsTable: Array<number[]>;

  // fix column width as defined in CSS (150px + 5px margin)
  boxWidth = 155;
  // calculated based on dynamic row width
  columnSize: number;

  getItemsTable(tableElement: Element): number[][] {
...
  }

  initTable() {
...
  }

  reorderDroppedItem(event: CdkDragDrop<number[]>) {
...
  }
```

Now let' start with the template

# Template

This is what we want:
```
- 'table' div: top element for table
  - 'row' divs: child elements for rows
    - 'column' divs: another sub-element for items
```

This is the template:
```
<div #tableElement cdkDropListGroup>
  <!-- Based on the width of template reference #tableElement' and item box width,
       columns per row can be calculated and a items table matrix is initialized-->
  <div
    fxLayout="row"
    *ngFor="let itemsRow of getItemsTable(tableElement)"
    cdkDropList
    cdkDropListOrientation="horizontal"
    [cdkDropListData]="itemsRow"
    (cdkDropListDropped)="reorderDroppedItem($event)"
  >
    <!-- Component.reorderDroppedItem():
         reorders table/view model, update input model, and resize table matrix-->
    <div *ngFor="let item of itemsRow" cdkDrag>
      <div class="drag-placeholder" *cdkDragPlaceholder></div>
      <div fxLayoutAlign="center center" class="item-box">{{ item }}</div>
    </div>
  </div>
</div>
```

In detail the template:
- tags the top element with a template reference called '#tableElement',
  - define table rows as child elements with **fxLayout="row"**
  - hook Component.reorderDroppedItem($event) for each draggable sub-element below
  - loop through Component.getItemsTable(tableElement) and create rows as child elements
    - fill each row and create additional row items as sub elements
      - attach for sub-elements a cdkDrag directive
      - define a drag-placeholder class

# CSS
The CSS looks like this:
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

.drag-placeholder {
  background: #ccc;
  border: dotted 3px #999;
  height: 150px;
  width: 50px;
  transition: transform 250ms cubic-bezier(0, 0, 0.2, 1);
}
```

On CSS side we defined an item-box class with a fixed width. Alternatively the fix width may be defined in the template e.g. using fxFlow="1 1 150px".

# Component

The component class holds these functions:
```
export class AppComponent {
...
  getItemsTable(tableElement: Element): number[][] {
    // calculate column size per row
    const { width } = tableElement.getBoundingClientRect();
    const columnSize = Math.round(width / this.boxWidth);
    // view has been resized? => update table with new column size
    if (columnSize != this.columnSize) {
      this.columnSize = columnSize;
      this.initTable();
    }
    return this.itemsTable;
  }

  initTable() {
    // create table rows based on input list
    // example: [1,2,3,4,5,6] => [ [1,2,3], [4,5,6] ]
    this.itemsTable = this.items
      .filter((_, outerIndex) => outerIndex % this.columnSize == 0) // create outter list of rows
      .map((
        _,
        rowIndex // fill each row from...
      ) =>
        this.items.slice(
          rowIndex * this.columnSize, // ... row start and
          rowIndex * this.columnSize + this.columnSize // ...row end
        )
      );
  }

  reorderDroppedItem(event: CdkDragDrop<number[]>) {
    // same row/container? => move item in same row
    if (event.previousContainer === event.container) {
      moveItemInArray(
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    } else {
      // different rows? => transfer item from one to another list
      transferArrayItem(
        event.previousContainer.data,
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    }

    // update items after drop: flatten matrix into list
    // example: [ [1,2,3], [4,5,6] ] => [1,2,3,4,5,6]
    this.items = this.itemsTable.reduce(
      (previous, current) => previous.concat(current),
      []
    );

    // re-initialize table - makes sure each row has same numbers of entries
    // example: [ [1,2], [3,4,5,6] ] => [ [1,2,3], [4,5,6] ]
    this.initTable();
  }
}
```

Basically the component does the following:
  - provides items table matrix as a view model
    - Component.getItemsTable(tableElement) creates everytime a new table, when table view is resized
    - table column size is dyncamically calculated based on
      - the width of the table element and
      - the fix width of an item (column)
  - Component.reorderDroppedItem($event) does 3 things
    - it re-orders the view model/table matrix,
    - updates input model by flattening the table/view model, and
    - re-arranges the view model

That's it!

Enjoy, Mr. T
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
... for the ones who wants to dive deeper....
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
# Solution - Explained in Detail

The main problem is the templates's flex row wrap dynamic behavior. I fixed that by removing this behavior - just using a flex row (without wrap) :).

Though there are examples of [transferring items between lists (using cdkDropListGroup)](https://material.angular.io/cdk/drag-drop/overview#transferring-items-between-lists), there is no solution of handling items in one list wrapped over multiple rows. 

So, how can we solve this? 

Answer: **The component keeps two models separated - one input model representing our items list, one view model representing a table matrix - and sync them with each other.**

So how can we map our component's item list into a table - a list of lists - and vice versa?

## Mapping a List (Array) Into a Table Matrix (Array of Arrays) and Vice Versa

The solution for that is quite simple. The idea is:
1. Mapping a one-dimensional list (represented by the input model) into a two-dimensional items table matrix (view model representing the flex row wrap) and vice versa,
2. Mapping a list of lists (of items) back into a flat list (of items).

Example:
1. list => list of lists.
   - Example: [1,2,3,4,5,6] => [ [1,2,3], [4,5,6] ]
   - Input model (a list of 6 elements) maps to a view model (a table with 2 rows, containing 3 column entries)
2. list of lists => list
   - Example: [ [1,2,3], [4,5,6] ] => [1,2,3,4,5,6]

For **1. list => list of lists** I use the Array functions filter, map, and slice. Example:
```
columnSize = 3;
> 3
items = [1,2,3,4,5,6,7]
> Array(7) [1, 2, 3, 4, 5, 6, 7]

table = items
  .filter((_, outerIndex) => outerIndex % this.columnSize == 0) // create outter list of rows
  .map((
    _,
    rowIndex // fill each row from...
  ) =>
    items.slice(
      rowIndex * this.columnSize, // ... row start to
      rowIndex * this.columnSize + this.columnSize // ...row end
    )
  )
> Array(3) [Array(3), Array(3), Array(1)] => [ [1,2,3], [4,5,6], [7]]
```

All I need is the column size - which dynamically varies on the view's table width. More on that below.

For **2. list of lists => list** I use reduce and concat. Example:
```
table.reduce(
  (previous, current) => previous.concat(current),
  []
)
> Array(7) [1, 2, 3, 4, 5, 6, 7]
```

## cdkDropListGroup to the rescue

Each table row is then grouped together using [cdkDropListGroup](https://material.angular.io/cdk/drag-drop/overview#transferring-items-between-lists). By grouping all rows together, it represents a logical unit of my input model.

The next challenge is:

How can I dynamically create an items table matrix, without knowing how many items will be shown per row?

For doing that I need to know two things:

1. What is the row's width?
2. What is a row item's (column) width?

In the template I have my template reference #tableElement. In the CSS I have defined an item-box class.

In this case the item-box's width has 155 pixels (width + margin). This together with the template reference - the top div element - the component can initialize a table matrix.

Dropping an item within the same list or to another list can be done by [moveItemInArray or transferArrayItem](https://material.angular.io/cdk/drag-drop/api#functions). The second and last method takes care of dropping the dragged item:

```
...
export class AppComponent {
...
  reorderDroppedItem(event: CdkDragDrop<number[]>) {
    // same row/container? => move item in same row
    if (event.previousContainer === event.container) {
      moveItemInArray(
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    } else {
      // different rows? => transfer item from one to another list
      transferArrayItem(
        event.previousContainer.data,
        event.container.data,
        event.previousIndex,
        event.currentIndex
      );
    }

    // update items after drop: flatten matrix into list
    // example: [ [1,2,3], [4,5,6] ] => [1,2,3,4,5,6]
    this.items = this.itemsTable.reduce((previous, current) =>
      previous.concat(current)
    );

    // re-initialize table - makes sure each row has same numbers of entries
    // example: [ [1,2], [3,4,5,6] ] => [ [1,2,3], [4,5,6] ]
    this.initTable();
  }
}
```

Now, that's really it :).

Happy coding, Mr. T