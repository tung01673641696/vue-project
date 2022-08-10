<template>
  <div class="list-posts">
    <div class="content_head">
      <h2>Danh sách bài viết</h2>
      <div class="content_head_button">
        <router-link to="/admin/create-post">
          <button class="button_add_product">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M12 6v6m0 0v6m0-6h6m-6 0H6"
              />
            </svg>
            <p>Thêm bài viết</p>
          </button>
        </router-link>
      </div>
    </div>

    <BaseTable
      v-bind="{ columns, rows, hasDeleteRow, hasUpdateRow, hasDetailRow }"
    ></BaseTable>

    <BasePagination :page="page" @update:page="page = $event" :total="total" />
  </div>
</template>

<script>
import BaseTable from "@/components/base/BaseTable.vue";
import BasePagination from "@/components/base/BasePagination.vue";
import { getAllNews } from "@/service/news.service.js";

export default {
  name: "ListPostsPage",
  components: { BaseTable, BasePagination },
  data() {
    return {
      hasDeleteRow: true,
      hasUpdateRow: true,
      hasDetailRow: false,
      page: 1,
      total: 0,
      columns: [
        {
          name: "title",
          align: "center",
          width: "200px",
          label: "Tiêu đề bài viết",
          type: "text",
        },
        {
          name: "content",
          align: "center",
          width: "400px",
          label: "Nội dung bài viết",
          type: "text",
        },
        {
          name: "image",
          align: "center",
          width: "200px",
          label: "Ảnh bài viết",
          type: "image",
        },
      ],
      rows: [],
    };
  },

  watch: {
    page(value) {
      this.getList(value);
    },
  },

  created() {
    this.getList();
  },

  methods: {
    /* paginate */
    updatePage(pageNumber) {
      this.page = pageNumber;
    },
    /* end of paginate */

    getList(page) {
      getAllNews(page)
        .then((response) => {
          this.rows = response.data.data.data;
          this.total = response.data.data.last_page;
        })
        .catch((error) => {
          throw new Error(error);
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.content_head {
  display: flex;
  justify-content: space-between;
  align-items: center;

  &_button {
    display: flex;
    align-items: center;
    gap: 10px;

    a {
      text-decoration: none;
    }

    .button_add_product {
      background-color: rgb(59, 130, 246);
      color: white;
      font-weight: 600;
      font-size: 11pt;
      padding: 8px 6px;
      border-radius: 5px;
      border: none;
      display: flex;
      gap: 5px;
      align-items: center;
      justify-content: center;

      svg {
        width: 25px;
        height: 25px;
      }

      p {
        text-decoration: none;
      }
    }

    .button_add_product:hover {
      background-color: rgb(37, 99, 235);
      cursor: pointer;
    }

    .button_add_size {
      background-color: rgb(59, 130, 246);
      color: white;
      font-weight: 600;
      font-size: 11pt;
      padding: 8px 6px;
      border-radius: 5px;
      border: none;
      display: flex;
      gap: 5px;
      align-items: center;
      justify-content: center;

      svg {
        width: 25px;
        height: 25px;
      }
    }

    .button_add_size:hover {
      background-color: rgb(37, 99, 235);
      cursor: pointer;
    }
  }
}
</style>
