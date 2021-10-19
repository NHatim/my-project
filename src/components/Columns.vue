<template>
  <div class="muuri-container">
    <h2>Kanban</h2>
    <div class="muuri-header filter-controls">
      <div class="control">
        Search
        <input
          class="search-field form-control"
          type="text"
          name="search"
          placeholder="Search"
        />
      </div>
      <!-- /.control -->

      <div class="control">
        Filter
        <select class="filter-field form-control">
          <option value="">ALL</option>
          <option value="red">Red</option>
          <option value="blue">Blue</option>
          <option value="green">Green</option>
        </select>
      </div>
      <!-- /.control -->

      <div class="control">
        Sort
        <select class="sort-field form-control">
          <option value="order">None</option>
          <option value="title">Ascendant</option>
        </select>
      </div>
      <!-- /.control -->
    </div>
    <!-- /.muuri-header -->

    <div class="grid">
      <div class="item blue w2 h2" data-color="blue" data-title="03">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 03</div>
        </div>
      </div>
      <!-- item -->

      <div class="item red w2" data-color="red" data-title="06">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 06</div>
        </div>
      </div>
      <!-- item -->

      <div class="item blue" data-color="blue" data-title="02">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 02</div>
        </div>
      </div>
      <!-- item -->

      <div class="item red" data-color="red" data-title="07">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 07</div>
        </div>
      </div>
      <!-- item -->

      <div class="item green h2" data-color="green" data-title="01">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 01</div>
        </div>
      </div>
      <!-- item -->

      <div class="item blue" data-color="blue" data-title="04">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 04</div>
        </div>
      </div>
      <!-- item -->

      <div class="item green w2" data-color="green" data-title="08">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 08</div>
        </div>
      </div>
      <!-- item -->

      <div class="item blue w2" data-color="blue" data-title="05">
        <div class="item-content">
          <!-- Safe zone, enter your custom markup -->
          <div class="custom-content">Item 05</div>
        </div>
      </div>
      <!-- item -->
      <!-- item -->
    </div>
    <!-- /.grid -->
  </div>
  <!-- /.muuri-container -->
</template>

<script>
import Muuri from 'muuri'
export default {
  name: "Columns",
  props: {
    msg: String,
  },

  setup() {
    document.addEventListener("DOMContentLoaded", function () {
      var grid = null,
        wrapper = document.querySelector(".muuri-container"),
        searchField = wrapper.querySelector(".search-field"),
        filterField = wrapper.querySelector(".filter-field"),
        sortField = wrapper.querySelector(".sort-field"),
        gridElem = wrapper.querySelector(".grid"),
        searchAttr = "data-title",
        filterAttr = "data-color",
        searchFieldValue,
        filterFieldValue,
        sortFieldValue,
        dragOrder = [];

      // Init the grid layout
      grid = new Muuri(gridElem, {
        dragEnabled: true,
      });

      // Set inital search query, active filter, active sort value and active layout.
      searchFieldValue = searchField.value.toLowerCase();
      filterFieldValue = filterField.value;
      sortFieldValue = sortField.value;

      // Search field event binding
      searchField.addEventListener("keyup", function () {
        var newSearch = searchField.value.toLowerCase();
        if (searchFieldValue !== newSearch) {
          searchFieldValue = newSearch;
          filter();
        }
      });

      // Filter field event binding
      filterField.addEventListener("change", filter);

      // Sort field event binding
      sortField.addEventListener("change", sort);

      // Filtering
      function filter() {
        filterFieldValue = filterField.value;
        grid.filter(function (item) {
          var element = item.getElement(),
            isSearchMatch = !searchFieldValue
              ? true
              : (element.getAttribute(searchAttr) || "")
                  .toLowerCase()
                  .indexOf(searchFieldValue) > -1,
            isFilterMatch = !filterFieldValue
              ? true
              : (element.getAttribute(filterAttr) || "") === filterFieldValue;
          return isSearchMatch && isFilterMatch;
        });
      }

      // Sorting
      function sort() {
        // Do nothing if sort value did not change.
        var currentSort = sortField.value;
        if (sortFieldValue === currentSort) {
          return;
        }

        // If we are changing from "order" sorting to something else
        // let's store the drag order.
        if (sortFieldValue === "order") {
          dragOrder = grid.getItems();
        }

        // Sort the items.
        grid.sort(
          currentSort === "title"
            ? compareItemTitle
            : currentSort === "color"
            ? compareItemColor
            : dragOrder
        );
        sortFieldValue = currentSort;
      }

      // Compare data-title
      function compareItemTitle(a, b) {
        var aVal = a.getElement().getAttribute(searchAttr) || "";
        var bVal = b.getElement().getAttribute(searchAttr) || "";
        return aVal < bVal ? -1 : aVal > bVal ? 1 : 0;
      }

      // Compare data-color
      function compareItemColor(a, b) {
        var aVal = a.getElement().getAttribute(filterAttr) || "";
        var bVal = b.getElement().getAttribute(filterAttr) || "";
        return aVal < bVal ? -1 : aVal > bVal ? 1 : compareItemTitle(a, b);
      }
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
$bg_color: #fff;
$font_color: #333;
$link_color: #999;
$link_hover_color: #777;

* {
  &:before,
  &:after {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
  }
}
body {
  background-color: $bg_color;
  color: $font_color;
  text-align: center;
  overflow-y: scroll;
  a,
  a:visited {
    color: $link_color;
    text-decoration: none;
  }
  a:hover {
    color: $link_hover_color;
  }
}
// ******************************
// End of Normalize
// ******************************

h1 {
  font-size: 36px;
  margin: 40px auto;
}
.filter-controls {
  text-align: center;
  margin-bottom: 30px;
  .control {
    display: inline-block;
    width: 240px;
    margin: 0 10px;
    * {
      box-sizing: border-box;
    }
    .form-control {
      width: 100%;
      height: 40px;
      padding: 0 20px;
      border: 2px solid #ccc;
      border-radius: 3px;
      background-color: #fff;
      color: #333;
      font-size: 16px;
      cursor: pointer;
      -webkit-appearance: none;
      appearance: none;
      &:focus {
        outline: 0;
        border-color: #0caaf5;
      }
      &:hover {
        opacity: 0.6;
      }
    }
  }
}

.grid {
  position: relative;
  max-width: 840px;
  margin: 0 auto;
}
.item {
  position: absolute;
  width: 200px;
  height: 200px;
  margin: 5px;
  z-index: 1;
  transition: transform 0.6s ease;
  cursor: move;
  &.blue {
    .custom-content {
      background-color: #33a6b8;
    }
  }
  &.red {
    .custom-content {
      background-color: #f19483;
    }
  }
  &.green {
    .custom-content {
      background-color: #86c166;
    }
  }
  &.w2 {
    width: 410px;
  }
  &.h2 {
    height: 410px;
  }
}
.item-content {
  position: relative;
  width: 100%;
  height: 100%;
  display: table;
}
.custom-content {
  display: table-cell;
  vertical-align: middle;
  text-align: center;
  background: #fff;
  color: #333;
  border-radius: 4px;
}

// If use dragging
.item.muuri-item-dragging {
  z-index: 3;
  transition: none;
}
.item.muuri-item-releasing {
  z-index: 2;
}

@media (max-width: 877px) {
  .item {
    width: calc(33.33% - 11px);
    height: calc(33.33vw - 11px);
    &.w2 {
      width: calc(33.33% - 11px);
    }
    &.h2 {
      height: calc(33.33vw - 11px);
    }
  }
}
@media (max-width: 640px) {
  .item {
    width: calc(50% - 10px);
    height: calc(50vw - 10px);
    &.w2 {
      width: calc(50% - 10px);
    }
    &.h2 {
      height: calc(50vw - 10px);
    }
  }
}
</style>
