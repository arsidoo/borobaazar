<template>
  <div class="col-md-2 mt-2 sidebar m-0 p-0 d-none d-md-block">
    <div class="btn-group d-flex justify-content-center text-center">
      <div class="border border-dark rounded m-1 p-2 bg-danger">
        <router-link :to="{ name: 'categories' }" class="text-white">
          <i class="mdi mdi-fish"></i> <br>
          Grocery
        </router-link>
      </div>
      <div class="border border-dark rounded m-1 p-2 bg-success">
        <router-link :to="{ name: 'brands' }" class="text-white">
          <i class="mdi mdi-tag"></i> <br>
          Brands
        </router-link>
      </div>
      <div class="border border-dark rounded m-1 p-2 bg-warning">
        <router-link :to="{ name: 'daily.deals' }" class="text-white">
          <i class="mdi mdi-handshake"></i> <br>
          Deals
        </router-link>
      </div>
    </div>
    <br>
    <ul>
      <li>
        <router-link :to="{ name: 'campaigns' }">
          <div class="category-item">
            <span class="category-title">
              <span class="text-secondary mdi mdi-view-dashboard-outline"></span>
              {{ lang.all_offers }}
            </span>
          </div>
        </router-link>
      </li>
      <li v-for="(category, index) in categories" :key="index">
        <router-link :to="{ name: 'product.by.category', params: { slug: category.slug } }">
          <div class="category-item" @click="toggleSubCategory(category)">
            <span class="category-title">
              <span v-if="category.icon" class="text-secondary" :class="category.icon"></span>
              <span v-else class="text-secondary mdi mdi-view-dashboard-outline"></span>
              {{ category.title }}
            </span>
            <span v-if="category.child_categories.length > 0" class="arrow-icon">
              <i class="fa" :class="category.showSubCategory ? 'fa-angle-double-down' : 'fa-angle-double-right'"></i>
            </span>
          </div>
          <ul v-if="category.showSubCategory">
            <li v-for="(subCategory, subIndex) in category.child_categories" :key="subIndex">
              <router-link :to="{ name: 'product.by.category', params: { slug: subCategory.slug } }">
                <div class="sub-category-item" @click="toggleSubSubCategory(subCategory)">
                  <span class="sub-category-title">{{ subCategory.title }}</span>
                  <span v-if="subCategory.categories.length > 0" class="arrow-icon">
                    <i class="fa" :class="subCategory.showSubSubCategory ? 'fa-chevron-down' : 'fa-chevron-right'"></i>
                  </span>
                </div>
                <ul v-if="subCategory.showSubSubCategory">
                  <li v-for="(subSubCategory, subSubIndex) in subCategory.categories" :key="subSubIndex">
                    <router-link :to="{ name: 'product.by.category', params: { slug: subSubCategory.slug } }">
                      <div class="sub-sub-category-item">
                        <span class="sub-sub-category-title">{{ subSubCategory.title }}</span>
                      </div>
                    </router-link>
                  </li>
                </ul>
              </router-link>
            </li>
          </ul>
        </router-link>
      </li>
      <li v-if="!addons.includes('ishopet')">
        <router-link :to="{ name: 'categories' }">
          <div class="category-item">
            <span class="category-title text-warning">
              <span class="mdi mdi-view-dashboard-outline"></span>
              {{ lang.view_all_categories }}
            </span>
          </div>
        </router-link>
      </li>
    </ul>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      categories: [],
    };
  },
  methods: {
    loadCategory() {
      let url = this.getUrl("sidebar/categories");
      axios.get(url).then(({ data }) => {
        this.categories = data.categories;
      });
    },
    toggleSubCategory(category) {
      this.$set(category, "showSubCategory", !category.showSubCategory);
    },
    toggleSubSubCategory(subCategory) {
      this.$set(subCategory, "showSubSubCategory", !subCategory.showSubSubCategory);
    },
  },
  created() {
    this.loadCategory();
  },
};
</script>
  
<style scoped>
/* Add some colors and styles to improve the appearance */
.sidebar {
  top: 0;
  height: 100vh;
  overflow-y: auto;
  background-color: #f9f9f9;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-right: 1px solid #ccc;
}

.sidebar ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.sidebar li {
  cursor: pointer;
}

.category-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 7px 7px 7px 25px;
  color: #000;
  background-color: #f2f2f2;
  transition: background-color 0.2s;
}

.category-item:hover {
  background-image: linear-gradient(to right, #f2f2f2, #a0a0a0);
}

.sub-category-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 20px;
  color: #555;
  background-color: #f6f8d9;

}

.sub-category-item:hover {
  background-image: linear-gradient(to right, #f2f2f2, #a0a0a0);
}

.sub-sub-category-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 28px;
  color: #000;
  background-color: #fff;
}

.sub-sub-category-item:hover {
  background-image: linear-gradient(to right, #f2f2f2, #a0a0a0);
}

.arrow-icon {
  font-size: 14px;
}

/* Style the submenus */
.sidebar ul ul {
  padding-left: 20px;
}

/* Style the submenus of submenus */
.sidebar ul ul ul {
  padding-left: 28px;
}

.sidebar li:hover {
  background-color: #eaeaea;
}

.sidebar li.active {
  background-color: #d3d3d3;
}

.arrow-icon {
  font-size: 14px;
  color: #555;
}

.arrow-icon:hover {
  color: #007bff;
}
</style>