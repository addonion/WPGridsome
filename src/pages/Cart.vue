<template>
  <Layout class="shape">
    <div class="container">
      <PageTitle title="Cart" />

      <div class="flex justify-center overflow-visible">

        <div class="px-8 py-4 bg-white border-b border-indigo-200 shadow-lg dark:bg-gray-700 dark:border-gray-800" style="min-width: 800px">
          <div v-if="cart !== null">
            <ul class="divide-y divide-indigo-50 dark:divide-gray-800 items">
              <li v-for="{node} in cart.cart.contents.edges" :key="node.product.node.id" class="flex items-center py-4">
                <img class="mr-8 border-2 border-white shadow dark:border-gray-700" width="64" :src="node.product.node.image.sourceUrl">
                <div>
                  <div>{{node.product.node.name}}</div>
                  <div class="text-sm text-indigo-500 dark:text-indigo-300">{{node.product.node.price}}</div>
                </div>
              </li>
            </ul>

          </div>
          <div v-else>
            <!-- Loading Icon  -->
            <svg class="w-20" version="1.1" id="L4" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px" viewBox="0 0 100 100" enable-background="new 0 0 0 0" xml:space="preserve">
              <circle fill="#111" stroke="none" cx="6" cy="50" r="6">
                <animate attributeName="opacity" dur="1s" values="0;1;0" repeatCount="indefinite" begin="0.1" />
              </circle>
              <circle fill="#111" stroke="none" cx="26" cy="50" r="6">
                <animate attributeName="opacity" dur="1s" values="0;1;0" repeatCount="indefinite" begin="0.2" />
              </circle>
              <circle fill="#111" stroke="none" cx="46" cy="50" r="6">
                <animate attributeName="opacity" dur="1s" values="0;1;0" repeatCount="indefinite" begin="0.3" />
              </circle>
            </svg>
          </div>
        </div>

      </div>

    </div>
  </Layout>
</template>


<script>
import { runMutation } from "../mixins/runMutation";

export default {
  data() {
    return {
      cart: null,
    };
  },
  mixins: [runMutation],
  methods: {
    async getCartItems() {
      const res = await this.runMutation(
        `query getcart {
            cart {
                isEmpty
                total
                contents {
                itemCount
                edges {
                    node {
                    product {
                        node {
                        id
                        name
                        image {
                            sourceUrl(size: SHOP_THUMBNAIL)
                        }
                        ... on SimpleProduct {
                            price(format: FORMATTED)
                        }
                        }
                    }
                    total
                    }
                }
                productCount
                }
            }
         }`
      );
      console.log(res);
      this.cart = await res.data.data;
    },
  },
  mounted() {
    this.getCartItems();
  },
};
</script>