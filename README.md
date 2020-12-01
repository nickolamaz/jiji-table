# Installation
1. Install [Yarn](https://yarnpkg.com/en/docs/install)
2. Install all packages using
   `yarn install` command

# Development
`yarn serve`

# Buld
`yarn build`

# Features
1. Search by keyword in table columns
2. Sort table columns by clicking on column header
3. Edit table cells
4. Paginate table using pre-defined page size (`10` by default)

# Usage

Just import component and apply it to your `components` like this:

```javascript
import TableWrapper from '@/components/TableWrapper.vue'

...

components: {
   TableWrapper
}
```

In your `.vue` file call:

```vue
<TableWrapper :table-data="tableData" :page-size="10"/>
```

Your `table-data` json should contain `titles` object with described table header defined by `code`: `title`. And `items` containing array of table row objects. Ex:

```json
{
   "titles": {
      "name": "Name",
      "height": "Height",
      "mass": "Mass",
      "hair_color": "Hair color",
      "skin_color": "Skin color",
      "eye_color": "Eye color",
      "birth_year": "Birth year",
      "gender": "Gender"
   },
   "items": [
      {
         "name": "Luke Skywalker",
         "height": "172",
         "mass": "77",
         "hair_color": "blond",
         "skin_color": "fair",
         "eye_color": "blue",
         "birth_year": "19BBY",
         "gender": "male"
      },
      {
         "name": "C-3PO",
         "height": "167",
         "mass": "75",
         "hair_color": "n/a",
         "skin_color": "gold",
         "eye_color": "yellow",
         "birth_year": "112BBY",
         "gender": "n/a"
      }
   ]
}
```
