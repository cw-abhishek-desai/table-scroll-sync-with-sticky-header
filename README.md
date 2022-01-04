# Table Scroll Sync with Sticky Header

## Summary


Build a scrollable table and sync it with multiple components and to create some sticky header within scrollable components for a page:

- Scrollable table structure.
- Sync scrollable components using `react-scroll-sync`.
- Make headers sticky in scrollable components.


## Implementation


## Scrollable table structure 

To make the table scrollable make `table` & `tbody` as `display: block;`, make `tbody` as scrollable element by `overflow-x-auto;` and give specific. width to table data cell(`td`) for.eg. `min-width: 240px;`. This will make the table scrollable.

Note: Also use `scrollbar-none::-webkit-scrollbar` & `scrollbar-none` if scrollbar needs to be hidden
Note: Do not change the table data cell(`td`) display-type. If changed it won't scroll.


####  Example.

```jsx
<StyleSheet style={commonStyles} />
<Table className={commonStyles.table}>
   <TableMeta type="body" className={`overflow-x-auto ${commonStyles.tableBody}`}>  //given oxygen classes for simplicity purpose
       <Table.Row>
               <td className={commonStyles.tableData}></td>
               <td className={commonStyles.tableData}></td>
               <td className={commonStyles.tableData}></td>
       </Table.Row>
   </TableMeta>
</Table>
```

```css
// CSS

.table {
    display: block;
}

.tableBody {
    display: block;
}

.tableData {
    min-width: 240px;
}
```

> Note : Since, the table are oxygen components they already have `display:table` applied to overwrite it use we would require module.scss classes instead of oxygen classes to overwrite it.




## Table Scroll Sync with other components 

To synronously scroll multiple `tables`/`containers` we need to use `react-scroll-sync` to achieve it. To use ScrollSync you have to wrap your scrollable content (ensure that you have `overflow: auto/scroll` in CSS) in `<ScrollSyncPane>` and then wrap everything in `<ScrollSync>`.

> Note : Add the `react-scroll-sync` in PWA project only.
> Note: Do not change the table data cell(`td`) display-type. If changed it won't scroll.
####  Example.
```jsx
import { ScrollSync, ScrollSyncPane } from "react-scroll-sync";

<ScrollSync proportional={false} vertical={false}> // use this props for smoother sync
  <>
    ...
    .....
    
    <Table className={commonStyles.table}>
       <ScrollSyncPane>
         <TableMeta type="body" className={`overflow-x-auto ${commonStyles.tableBody}`}>  
             ....
               ....
                  <td className={commonStyles.tableData}></td>
               ....
             .....
         </TableMeta>
       </ScrollSyncPane>
    </Table>
    ...
    ....
    <Table className={commonStyles.table}>
       <ScrollSyncPane>
         <TableMeta type="body" className={`overflow-x-auto ${commonStyles.tableBody}`}>  
             ....
               ....
                  <td className={commonStyles.tableData}></td>
               ....
             .....
         </TableMeta>
       </ScrollSyncPane>
    </Table>
    ....
    ...
  </>
</ScrollSync>
```

> Note : `ScrollSync` requires minimum 1 or greater than 1 `ScrollSyncPane` inside if not found it will give error's so only keep `ScrollSync` if there are `ScrollSyncPane` inside the children

For more info on lib Click here : [`react-scroll-sync`](https://github.com/okonet/react-scroll-sync)




## Sticky Component inside a scrollable table Structure

This is the most trickest part to achieve since `overflow` and `sticky` don't go well hand in hand, And to top it all of we have the `table` structure to handle. To achieve this we have to do some changes to the structure of the table where we need to make `tr` table row as block & `td` as block too. And then apply this css to `tr`.
```css
tr {
  display: block;
  position: sticky;
  left: 0px;       // to stick to the left side of the table.
  width: 100vw;    // here since we require it to be responsive and cover the whole horizontal axis we have use `100vw`
  min-width: 100vw;
}

td {
  display: block;  //we need it to be block or else it will only cover the area which is set by min-width above.
```



##  Example with all sections clubbed together.


```js
import { ScrollSync, ScrollSyncPane } from "react-scroll-sync";

<ScrollSync proportional={false} vertical={false}>
  <>
    ...
    .....
    
    <Table className={commonStyles.table}>
       <ScrollSyncPane>
         <TableMeta type="body" className={`overflow-x-auto ${commonStyles.tableBody}`}> 
           
            // Sticky Row added specific extra classes
            <Table.Row className={`${styles.itemStickyTabelRow} ${comparisonTableClasses.rowClasses}`}>
              <td className={`${styles.itemStickyTableCell} ${comparisonTableClasses.itemTableClasses}`}>
                ...
              </td>
            </Table.Row>
           
           // !Scrollable Row See to it that  `tr` & `td` are `display: table-cell;`
             <Table.Row className={comparisonTableClasses.rowClasses}>
              <td className={comparisonTableClasses.itemTableClasses}>
                ...
              </td>
            </Table.Row>
                  <td className={commonStyles.tableData}></td>

         </TableMeta>
       </ScrollSyncPane>
    </Table>
    ...
    ....
    <Table className={commonStyles.table}>
       <ScrollSyncPane>
         <TableMeta type="body" className={`overflow-x-auto ${commonStyles.tableBody}`}>  
             ....
               ....
                  <td className={commonStyles.tableData}></td>
               ....
             .....
         </TableMeta>
       </ScrollSyncPane>
    </Table>
    ....
    ...
  </>
</ScrollSync>
```

```css
// CSS
.table {
    display: block;
}

.tableBody {
    display: block;
}

.tableData {
    min-width: 240px;
}

.itemStickyTabelRow {
  display: block;
  position: sticky;
  left: 0px;      
  width: 100vw;    
  min-width: 100vw;
}

.itemStickyTableCell {
  display:block;
}
```

## Reference

- PR: https://github.com/carwale/carwaleweb/pull/39079
- Ref: https://elad.medium.com/css-position-sticky-how-it-really-works-54cd01dc2d46




