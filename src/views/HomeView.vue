<template>
  <main>
    <div class="chat chat-start">
      <div class="chat-bubble">TA用网易云音乐在班群分享了一首歌，<br>好好听！好想知道TA的音乐账号！<br>这样就有更多话题了。</div>
    </div>
    <div class="chat chat-end">
      <div class="chat-bubble">
        这还不简单，复制TA发的音乐链接，<br>
        到<a href="https://findneteasecloudmusicuser.nclgclub.com/" target="_blank">这个网站</a>，
        就能找得到TA的账号主页啦～
      </div>
    </div>

    <div class="chat chat-start">
      <div class="chat-bubble">真的吗，我现在就去试试看！</div>
    </div>
    <div class="chat chat-end">
      <div class="chat-bubble">
        不论是聊天中的分享链接，<br>还是分享在朋友圈或者微博的，<br>
        都可以解析喔。
      </div>
    </div>

    <div class="chat chat-start">
      <div class="chat-bubble">すごい！你太厉害了！</div>
    </div>
    <div class="chat chat-end">
      <div class="chat-bubble">
        快去试试呀，等你好消息！<br>
        有问题的话可以直接在<label for="my-modal" class="btn btn-xs btn-success">作者公众号</label>寻求帮助喔。
        <input type="checkbox" id="my-modal" class="modal-toggle" />
        <div class="modal">
          <div class="modal-box">
            <img src="../assets/qr_code.png">
            <div class="modal-action">
              <label for="my-modal" class="btn btn-sm">
                好
              </label>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>
