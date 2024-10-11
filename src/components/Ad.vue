<template>
    <div class="crad-box">
        <!-- 双图广告位 -->
        <div class="ad2">
            <a class="left" href="#">
                <img src="/images/ad.png" alt="广告位">
            </a>
            <a class="right" href="#">
                <img src="/images/ad2.png" alt="广告位">
            </a>
        </div>
        <!-- 文字广告位 -->
        <div class="ad3">
            <div class="ad3-box">
                <a href="#">文字广告位</a>
            </div>
            <div class="ad3-box">
                <a href="#">文字广告位</a>
            </div>
            <div class="ad3-box">
                <a href="#">文字广告位</a>
            </div>
            <div class="ad3-box">
                <a href="#">文字广告位</a>
            </div>
        </div>
    </div>
</template>

<script setup>

</script>

<style scoped>
p {
    font-size: 15px;
}

.crad-box {
    transition: all 0.2s ease-in-out;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    flex-wrap: wrap;
    margin-top: 12px;
    background-color: var(--color-bg-2);
    padding: 12px;
    border-radius: var(--fillet);
    box-shadow: var(--shadow);
}

.ad2 {
    width: 100%;
    display: flex;
    justify-content: space-between;
}

.ad2 img {
    width: 100%;
    height: auto;
    border-radius: var(--fillet);
}

.ad2 .left {
    margin-right: 8px;
    width: 50%;
}

.ad2 .right {
    width: 50%;
}

.ad3 {
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}

.ad3 a {
    color: var(--color-text-1);
    transition: color 0.5s ease;
}

.ad3 a:hover {
    color: #ff765a;
    /* 鼠标悬停时的颜色 */
}

.ad3-box {
    margin-top: 8px;
    border-radius: var(--fillet);
    width: 24%;
    padding: 10px;
    border: 2px dotted #5a65ff;
    text-align: center;
}

@media only screen and (max-width:768px) {
    .ad3-box {
        width: 49%;
        border: 1px dotted #5a65ff;
    }
}
</style>
