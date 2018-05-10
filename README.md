
## Available props

| Prop                  | Type            | Default     | Description                              |
|-----------------------|-----------------|-------------|------------------------------------------|
| currentPage                 | Number    |      1     | Current page             |
| totalCount                  | Number          |   0        | Total number of records will be paginated                 |
| perPage                  | Number          |    1         | Number of records in one page                      |
| limit                  | Number          |    10         | Number of pages will be show every chunk pages               |
| type                  | String          |    df         | Type of pagination (`df or simple`)                 |

## Events

| Event | Params | Description |
|-------|--------|-------------|
|change| value of new page | Go to new page |

## Usage

```
// template
<pagination
  :current-page="currentPage"
  :total-count="totalCount"
  :per-page="perPage"
  v-model="currentPage"
  @change="handleChangePagination($event)"
>
</pagination>
```

```
// script
handleChangePagination (event) {
  this.currentPage = event
  console.log(`Go to page ${event}`)
  // Load new data
}
```

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

For detailed explanation on how things work, consult the [docs for vue-loader](http://vuejs.github.io/vue-loader).
