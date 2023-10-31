<template>
    <div class="flex flex-col justify-center items-center">
        <div id="vcomments"></div>
    </div>
</template>

<script>
import Valine from 'valine';

export default {
    mounted() {
        new Valine({
            el: '#vcomments',

            placeholder: "欢迎你来到这里，同时也欢迎你在这里畅所欲言，分享自己的故事~\n留言请尽量使用QQ邮箱（站内已经处理隐私，他人不可见），防止收不到回复。",
            meta: ['nick', 'mail', 'link'],
            recordIP: true,
            enableQQ: true,
            requiredFields: ['nick', 'mail', 'link']
        })
    },
};
</script>