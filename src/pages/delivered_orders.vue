<template>
    <div>
        <div v-for="order in orders" :key="order.id">
            <b-card
                border-variant="primary"
                :header="`تاريخ الطلب: ${order.orderDate}`"
                header-bg-variant="primary"
                header-text-variant="white"
                align="center"
            >
                <b-card-text>
                    <div>
                        <p>وسيلة الدفع</p>
                        <p>
                            {{ order.paymentMethod.textEn }} |
                            {{ order.paymentMethod.textAR }}
                        </p>
                    </div>
                    <hr />
                    <div>
                        <p>مكان التسوق</p>
                        <p>
                            {{ order.shippingAddress.addressText }}
                        </p>
                    </div>
                    <hr />
                    <div>
                        <p>المستخدم</p>
                        <b-card>
                            <div class="text-center">
                                <b-img
                                    width="100%"
                                    v-if="order.user.imageUrl"
                                    :src="order.user.imageUrl"
                                    alt="User image"
                                    fluid
                                ></b-img>
                            </div>
                            <div class="card-body">
                                <h5 class="card-title">
                                    {{ order.user.fullName }}
                                </h5>
                                <h6 class="card-subtitle">
                                    {{ order.user.email }} |
                                    {{ order.user.phone }}
                                </h6>
                                <br />
                            </div>
                        </b-card>
                    </div>
                    <hr />
                    <div>
                        <p>فتى التوصيل</p>
                        <b-card>
                            <div class="text-center">
                                <b-img
                                    width="100%"
                                    v-if="order.deliveryBoy.imageUrl"
                                    :src="order.deliveryBoy.imageUrl"
                                    alt="Delivery boy image"
                                    fluid
                                ></b-img>
                            </div>
                            <div class="card-body">
                                <h5 class="card-title">
                                    {{ order.deliveryBoy.fullName }}
                                </h5>
                                <h6 class="card-subtitle">
                                    {{ order.deliveryBoy.email }} |
                                    {{ order.deliveryBoy.phone }}
                                </h6>
                                <br />
                            </div>
                        </b-card>
                    </div>
                    <hr />
                    <div>
                        <p>المنتجات في العربة</p>
                        <b-card
                            v-for="product in order.cartItemList"
                            :key="product.id"
                        >
                            <div class="row">
                                <div class="col-md-4">
                                    <b-card
                                        :title="
                                            product.product.titleAr
                                                ? `${product.product.title} | ${product.product.titleAr}`
                                                : `${product.product.title}`
                                        "
                                        :img-src="
                                            product.product.imageUrl
                                                ? product.product.imageUrl
                                                : ''
                                        "
                                        img-alt="Image"
                                        img-top
                                        style="max-width: 20rem;"
                                        class="mb-2"
                                    >
                                        <b-card-text>
                                            {{ product.product.description }}
                                        </b-card-text>

                                        <b-button variant="primary"
                                            >الكمية: {{ product.qty }}</b-button
                                        >
                                    </b-card>
                                </div>
                            </div>
                        </b-card>
                    </div>
                    <hr />
                    <div>
                        <p>القسيمة</p>
                        <p>
                            {{
                                order.coupon
                                    ? `${order.coupon.code} | value: ${order.coupon.value}`
                                    : "No Cuopon"
                            }}
                        </p>
                    </div>
                    <hr />
                    <div>
                        <p>رسوم التوصيل</p>
                        <p>
                            {{ order.deliveryFees }}
                        </p>
                    </div>
                    <hr />
                    <div>
                        <p>المجموع الكلي للطلب</p>
                        <p>
                            {{ order.orderTotal }}
                        </p>
                    </div>
                    <hr />
                    <div>
                        <p>المجموع الجزئي للطلب</p>
                        <p>
                            {{ order.partialTotal }}
                        </p>
                    </div>
                    <hr />
                </b-card-text>
            </b-card>
        </div>
        <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="perPage"
            @change="getNew"
            class="mt-5"
        ></b-pagination>
    </div>
</template>

<script>
export default {
    data() {
        return {
            orders: [],
            currentPage: 1,
            rows: 0,
            perPage: 20
        };
    },
    async mounted() {
        this.getNew(1);
    },
    methods: {
        async getNew(e) {
            await this.$http
                .get(`/admin/deliveredOrders?page=${e - 1}`, {
                    headers: {
                        Authorization: `Bearer ${localStorage.getItem("token")}`
                    }
                })
                .then(res => {
                    this.rows = res.data.totalElements;
                    this.orders = res.data.content;
                    document.body.scrollTop = 0;
                    document.documentElement.scrollTop = 0;
                });
        }
    }
};
</script>

<style></style>
