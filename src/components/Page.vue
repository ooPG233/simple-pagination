<template>
    <div class="page">
        <div class="pageDescription">
            <p>
                <!-- 此处插入描述 -->
                共有 {{ pageLength }} 页
                <slot></slot>
            </p>
        </div>
        <div class="switchTo">
            <input
                type="text"
                name=""
                @input="inputLimit"
                v-model="switchNumber"
            />
            <div @click="switchTo">Go</div>
        </div>
        <ul class="pageButton">
            <li @click="showPrepage" v-show="currentPageIndex !== 0">
                <img src="../assets/images/prepage.png" alt="" />
            </li>
            <div
                class="pageButton-outerBox"
                :style="{ width: outerBoxWidth + 'px' }"
            >
                <div
                    class="pageButton-innerBox"
                    :style="{
                        width: innerBoxWidth + 'px',
                        'margin-left': offsetX + 'px',
                    }"
                >
                    <li
                        v-for="index in pageList.keys()"
                        :key="index"
                        :class="{ pageFocus: pageList[index] }"
                        @click="switchPage(index)"
                    >
                        {{ index + 1 }}
                    </li>
                </div>
            </div>
            <li
                @click="showNextpage"
                v-show="currentPageIndex !== lastPage - 1"
            >
                <img src="../assets/images/nextpage.png" alt="" />
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: "Page",
    data() {
        return {
            pageList: [],
            innerBoxWidth: 0,
            outerBoxWidth: 0,
            lastPage: 0,
            currentPageIndex: 0,
            offsetX: 0,
            switchNumber: "",
        };
    },
    methods: {
        showPrepage() {
            if (this.currentPageIndex > 0) {
                this.switchPage(this.currentPageIndex - 1);
            }
        },
        showNextpage() {
            if (this.currentPageIndex < this.lastPage - 1) {
                this.switchPage(this.currentPageIndex + 1);
            }
        },
        switchPage(pageIndex) {
            if (this.currentPageIndex == pageIndex) {
                return;
            }
            this.switchPageFocus(this.currentPageIndex); //切换样式
            if (this.lastPage > 5) {
                if (pageIndex > 1 && pageIndex < this.lastPage - 2) {
                    //如果不是最前与最后两个页按钮，则将当前选中的页移到中间
                    this.offsetX = -(pageIndex - 2) * 40;
                } else if (pageIndex <= 1) {
                    this.offsetX = 0;
                } else {
                    this.offsetX = -(this.lastPage - 5) * 40;
                }
            }
            this.switchPageFocus(pageIndex);
            this.currentPageIndex = pageIndex;
            this.$emit("switchPage", pageIndex + 1); //将点击事件注册
        },
        switchPageFocus(pageIndex) {
            if (this.pageList[pageIndex]) {
                this.$set(this.pageList, pageIndex, false);
            } else {
                this.$set(this.pageList, pageIndex, true);
            }
        },
        inputLimit() {
            //限制跳转输入框的输入字符
            this.switchNumber = (this.switchNumber + "").replace(/[^\d]/g, ""); //正则替换掉非数字的输入字符串
            this.switchNumber = this.switchNumber < 1 ? "" : this.switchNumber;
            this.switchNumber =
                this.switchNumber > this.pageLength
                    ? this.pageLength
                    : this.switchNumber;
        },
        switchTo() {
            if (this.switchNumber) {
                //如果值不为空
                this.switchPage(this.switchNumber - 1);
                this.switchNumber = "";
            }
        },
    },
    props: {
        pageLength: Number, //接收一个数值类型的参数作为页数
    },
    mounted: function () {
        let list = [];
        for (let i = 0; i < this.pageLength; i++) {
            list[i] = false;
        }
        this.pageList = [...list];
        this.lastPage = this.pageList.length;
        this.innerBoxWidth = this.lastPage * 40 - 8;
        this.pageList[this.currentPageIndex] = true;
        this.lastPage * 40 - 8 > 192
            ? (this.outerBoxWidth = 192)
            : (this.outerBoxWidth = this.lastPage * 40 - 8);
    },
    watch: {
        pageLength() {
            let list = [];
            for (let i = 0; i < this.pageLength; i++) {
                list[i] = false;
            }
            this.pageList = [...list];
            this.lastPage = this.pageList.length;
            this.innerBoxWidth = this.lastPage * 40 - 8;
            this.pageList[this.currentPageIndex] = true;
            this.lastPage * 40 - 8 > 192
                ? (this.outerBoxWidth = 192)
                : (this.outerBoxWidth = this.lastPage * 40 - 8);
        },
    },
};
</script>

<style lang='less' scoped>
@baseColor:rgba(24, 144, 255, 1);
*{
    margin:0;
    padding:0;
}
.page {
    display: inline-block;
    height: 32px;
    .switchTo {
        display: inline-flex;
        vertical-align: top;
        margin-right: 10px;
        justify-content: space-around;
        width: 80px;
        height: 32px;
        input {
            width: 30px;
            height: 30px;
            display: inline-block;
            text-align: center;
            font-size: 14px;
            border: 1px solid #d9d9d9;
            border-radius: 6px;
            outline: none;
        }
        div {
            cursor: pointer;
            width: 30px;
            height: 30px;
            color: black;
            background-color: white;
            border-radius: 6px;
            border: 1px solid #d9d9d9;
            line-height: 30px;
            font-size: 14px;
            text-align: center;
            &:hover {
                color: @baseColor;
            }
        }
    }
    .pageDescription {
        display: inline-block;
        vertical-align: top;
        margin-right: 12px;
        height: 32px;
        line-height: 32px;
        p {
            height: 32px;
            line-height: 32px;
        }
    }
    .pageButton {
        display: inline-block;
        height: 32px;
        li {
            display: inline-block;
            font-size: 14px;
            width: 30px;
            height: 30px;
            line-height: 30px;
            border: 1px solid #d9d9d9;
            border-radius: 6px;
            text-align: center;
            vertical-align: top;
            cursor: pointer;
            background-color: white;
            img {
                width: 14px;
                height: 14px;
                display: inline-block;
                margin: 8px auto;
                opacity: 0.4;
            }
        }
        .pageButton-outerBox {
            display: inline-block;
            overflow: hidden;
            width: 192px;
            margin: 0 8px;
            position: relative;
            .pageButton-innerBox {
                display: inline-flex;
                overflow: hidden;
                justify-content: space-between;
                position: relative;
                margin-left: -200px;
                transition: 0.2s;
                li {
                    width: 30px;
                    height: 30px;
                    line-height: 30px;
                    border: 1px solid #d9d9d9;
                    border-radius: 6px;
                    text-align: center;
                    &:hover {
                        color: @baseColor;
                    }
                }
                .pageFocus {
                    color: white;
                    background-color: @baseColor;
                    border-color: @baseColor;
                    cursor: default;
                    &:hover {
                        color: white;
                    }
                }
            }
        }
    }
}
</style>