<template>
    <view>
        <cu-custom bgColor="bg-gradual-blue"><block slot="content">添加</block></cu-custom>
        <view class="" style="margin-top: 40%">
            <view class="flex justify-center margin-bottom-lg"><image src="../../static/scan-barcode.png" mode="widthFix" class="scan-img"></image></view>
            <view class="flex justify-center"><button class="cu-btn round lg bg-cyan" @click="scan">点击扫码添加</button></view>
        </view>
    </view>
</template>

<script>
export default {
    data() {
        return {
            modalName: null,
            booklist: ''
        };
    },
    onLoad() {
        console.log('Add Load');
    },
    onShow() {
        console.log('Add Load');
        this.booklist = uni.getStorageSync('booklist');
    },
    methods: {
        scan(e) {
            var that = this;
            uni.scanCode({
                onlyFromCamera: true,
                success: function(res) {
                    console.log(res.scanType, res.result);
                    uni.request({
                        url: 'https://api.douban.com/v2/book/isbn/:' + res.result,
                        data: { apikey: '0df993c66c0c636e29ecbb5344252a4a' },
                        header: {
                            'content-type': 'application/x-www-form-urlencoded'
                        },
                        success: res => {
                            if (res.statusCode == 200) {
                                var book = {
                                    title: res.data.title,
                                    author: res.data.author,
                                    image: res.data.image,
                                    pubilsher: res.data.publisher,
                                    price: res.data.price,
                                    pubdate: res.data.pubdate
                                };
                                that.booklist.push(book);
                                uni.setStorageSync('booklist', that.booklist);
                                uni.showToast({
                                    title: '添加成功',
                                    icon: 'success'
                                });
                            } else {
                                uni.showToast({
                                    title: '书籍无法识别',
                                    icon: 'none'
                                });
                            }
                        }
                    });
                }
            });
            // uni.request({
            //     url: 'https://api.douban.com/v2/book/isbn/:9787111562108',
            //     data: { apikey: '0df993c66c0c636e29ecbb5344252a4a' },
            //     header: {
            //         'content-type': 'application/x-www-form-urlencoded'
            //     },
            //     success: res => {
            //         if (res.statusCode == 200) {
            //             var book = {
            //                 title: res.data.title,
            //                 author: res.data.author,
            //                 image: res.data.image,
            //                 pubilsher: res.data.publisher,
            //                 price: res.data.price,
            //                 pubdate: res.data.pubdate
            //             };
            //             console.log(book);
            //             this.BookList.push(book);
            //             console.log(this.BookList);
            //             uni.showToast({
            //                 title: '添加成功',
            //                 icon: 'success'
            //             });
            //         } else {
            //             uni.showToast({
            //                 title: '书籍无法识别',
            //                 icon: 'none'
            //             });
            //         }
            //     }
            // });
        }
    }
};
</script>

<style>
.scan-img {
    width: 30%;
    height: 30%;
    margin-bottom: 25upx;
}
</style>
