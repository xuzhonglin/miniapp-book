<template>
    <view>
        <cu-custom bgColor="bg-gradual-blue"><block slot="content">列表</block></cu-custom>
        <!--        <view class="cu-bar bg-white solid-bottom">
            <view class="action">
                <text class="cuIcon-title text-orange "></text>
                消息列表
            </view>
        </view> -->
        <view class="cu-list menu-avatar">
            <view
                class="cu-item"
                :class="modalName == 'move-box-' + index ? 'move-cur' : ''"
                v-for="(item, index) in booklist"
                :key="index"
                @touchstart="ListTouchStart"
                @touchmove="ListTouchMove"
                @touchend="ListTouchEnd"
                :data-target="'move-box-' + index"
            >
                <view class="cu-avatar radius lg" :style="{ backgroundImage: 'url(' + item.image + ')' }"></view>
                <view class="content">
                    <view class="text-grey">{{ item.title }}</view>
                    <view class="text-gray text-sm">{{ item.author[0] }} {{ item.pubilsher }}</view>
                </view>
                <view class="action">
                    <view class="text-grey text-sm">{{ item.pubdate }}</view>
                    <view class="cu-tag round bg-grey sm">¥ {{ item.price }}</view>
                </view>
                <view class="move">
                    <!-- <view class="bg-blue">喜爱</view> -->
                    <view class="bg-red" :data-index="index" @tap="removeBook">删除</view>
                </view>
            </view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            modalName: null,
            listTouchStart: 0,
            listTouchDirection: null,
            booklist: ''
            // booklist: [
            //     {
            //         title: '新概念51单片机C语言教程',
            //         author: ['郭天祥'],
            //         image: 'https://img1.doubanio.com/view/subject/m/public/s6457519.jpg',
            //         pubilsher: '电子工业出版社',
            //         price: '69.00',
            //         pubdate: '2019-1'
            //     },
            //     {
            //         title: '新概念51单片机C语言教程',
            //         author: ['郭天祥'],
            //         image: 'https://img1.doubanio.com/view/subject/m/public/s6457519.jpg',
            //         pubilsher: '电子工业出版社',
            //         price: '69.00',
            //         pubdate: '2019-1'
            //     }
            // ]
        };
    },
    onLoad() {
        console.log('List Load');
        // this.booklist = uni.getStorageSync('booklist');
        // console.log(this.BookList);
        // console.log('get cahce success');
    },
    onShow() {
        console.log('List Show');
        this.booklist = uni.getStorageSync('booklist');
        console.log(this.booklist);
        console.log('get cahce success');
    },
    onHide() {
        // this.BookList = this.booklist;
        // console.log(this.BookList);
        console.log('List Hide');
    },
    methods: {
        removeBook(e) {
            var index = e.currentTarget.dataset.index;
            console.log(e.currentTarget.dataset.index);
            this.booklist.splice(index, 1);
            uni.setStorageSync('booklist', this.booklist);
        },
        // ListTouch触摸开始
        ListTouchStart(e) {
            this.listTouchStart = e.touches[0].pageX;
        },

        // ListTouch计算方向
        ListTouchMove(e) {
            this.listTouchDirection = e.touches[0].pageX - this.listTouchStart > 0 ? 'right' : 'left';
        },

        // ListTouch计算滚动
        ListTouchEnd(e) {
            if (this.listTouchDirection == 'left') {
                this.modalName = e.currentTarget.dataset.target;
            } else {
                this.modalName = null;
            }
            this.listTouchDirection = null;
        }
    }
};
</script>

<style></style>
