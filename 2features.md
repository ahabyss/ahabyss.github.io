---
layout: page
title : Features
tagline: ["Truth be told, I'm quite proud of my house blend. To attain my flavor and fragrance, I use five different types of coffee beans."]
tagimg: "https://twemoji.maxcdn.com/2/72x72/2615.png"
tagemoji: True
permalink: /features/
---

<h2>Features</h2>

<div class="manual-post">
 <div class="manual manual-title">
  <strong>Notes</strong>
 </div>
 <p>  
  <div class="manual-content">
   <ol>
    <li>Some anime were presented multiple times, to choose from these 'alternates' use the Alt menu</li> 
    <li>Some episodes weren't shown in their entirety (think intros/outros) the portions of an episode without chat show up darker in the seek bar. If you don't see chat playing check this!</li>
   </ol>
  </div>
 </p>
</div>

<div class="manual-post">
 <div class="manual manual-title">
 <strong>Options: Toggles</strong>
 </div>
 <p></p>
  <div class="manual-content">
 
  <ul>
  <li>Better Twitch TV emotes<img class='featureImg' src='https://cdn.betterttv.net/emote/566c9f6365dbbdab32ec0532/1x'></li>
  <dd>This will turn off global and any specified channel emotes</dd>
  <li>Timestamps<img class='featureImg' src='../assets/timestamps.png'></li>
  <dd>Same setting as Twitch, except these timestamps show minute:second</dd>
  <li>Emojis <div class='featureImg'><img class='bttvemoji' src='https://twemoji.maxcdn.com/2/72x72/1f6e1.png'> <img class='ttvemote' src='https://static-cdn.jtvnw.net/emoticons/v1/508661/1.0'> <img class='bttvemoji' src='https://twemoji.maxcdn.com/2/72x72/1f5e1.png'></div></li>
  <dd>TCR will automatcally convert :XX: type emojis with this setting</dd>
  <li>Badges</li>
  <dd>This option simply turns badges on. Overrustle only logs the users name and chat message, thus any badge data is 
  lost. TCR can randomly generate badges in an appropriate ratio to improve the 'feel' when this data cannot be recovered.
  <br>Marathon Status:</dd>
  <div class="dd">
  <dd>Anime Marathon 1: Random badges</dd>
  <dd>RWBY Marathon: Accurate badges</dd>
  </div>
  </ul>
  </div>
 <p></p>
</div>


<div class="manual-post">
 <div class="manual manual-title">
 <a name="vlc" class="inpagelink"><strong>Options: VLC</strong></a>
 </div>
 <p></p>
  <div class="manual-content">

   With long episodes it isn't too much bother to sync TCR with your video player manually. But with the RWBY marathon having far shorter 
   episodes with unpredictable start delay, the old way becomes annoying, and so I implemented the VLC connection.
    
    <video id="vlcvid" width="740" controls>
        <source src="https://giant.gfycat.com/PastelDeficientChinchilla.webm" type="video/webm">
    </video>
   
   <div style='margin-top:0.5em'>If VLC connection is enabled, when you press the TCR button in your browser it will check if VLC is running, and if it has the correct connection password. If either of these conditions aren't met you can continue which will disable the VLC option
   until you manually re-enable it in the options menu.</div>
   
   <div style='margin-top:0.5em'>The VLC connection is performed using <a href="https://wiki.videolan.org/VLC_HTTP_requests/">VLC http</a>. For security purposes you must enable this interface and set a password
   in your VLC player. 
   <dd>See the illustrative guide <a href="https://hobbyistsoftware.com/vlcsetup-win-manual">here</a> (note: I recommend doing it manually, also you can skip the firewall part because everything is local)</dd>
   The default password is TCR, but you
   should choose another one and be sure to input the new password into the TCR settings menu.</div>

  </div>
  <p></p>
 
</div>

<div class="manual-post">
 <div class="manual manual-title">
 <strong>Options: Other</strong>
 </div>
 <p></p>
  <div class="manual-content">

    <ul>
  <li>Better Twitch TV Channel Emotes</li>
  <dd>Sometimes popular BTTV emotes are used even if they aren't supported in the TwitchPresents channel. Or they are frankerZ emotes which TCR doesn't 
  support. To remedy this you can add twitch channels to this list and their BTTV channel emotes will be parsed like the regular emotes.</dd>
  <li>Username Highlight</li>
  <dd>This is a fun setting that will add a slight highlight to any usernames you add to the list. Add your name to get reminded of the dank memes you participated in.</dd>
  <li>Emote Filter</li>
  <dd>If you find any emotes particularly troublesome you can add them to this list and they will not be shown.</dd>
  </ul>

  </div>
  <p></p>
 
</div>

<div class="manual-post">
 <div class="manual manual-title">
 <strong>How does it work?</strong>
 </div>
 <p>  
  <div class="manual-content">
TCR contains a pre-formatted chat archive for each anime episode. It loads one of these files each time you begin playback and displays the messages with the same timing as they were written.
<br>
<div style='margin-top:0.5em'>Building these chat archives and getting the timing correct is the tricky part. Because TwitchPresents doesn't store vods Twitch doesn't serve chat logs. Instead TCR uses logs from <a href="https://overrustlelogs.net/">OverRustle</a> which captures chat in the TwitchPresents
channel. These logs must then be synchronized to each anime episode. </div>
<div style='margin-top:0.5em'>A lot of work goes into syncing each episode archive (thanks to <a href="https://twitch.tv/wulfy_zef">wulfy_zef</a> for his help with some anime). There are two main methods I've used depending on 
vod availiablity.</div>
  </div>
 </p> 
 <p>
  <div class="manual-content">
   <ol>
    <li>The first method involves a vod recording with chat on screen, here it is straightforward to determine the first
message of an episode and thus the timing. Most anime were shown with mid-episode breaks, and thus four timestamps are required for each episode. One at the beginning and end of each chat segment.
<div style='margin-top:0.25em'>A major drawback of this method is that the quality of the synchronization is limited by the quality of the vod, some vods lag up to 20 seconds. A second limitation is that getting the timestamps requires a 
vod that shows chat. Without a complete vod getting timestamps that correspond to the key points of each episode isn't possible. Fortunatly recordings exist of most of the 1st marathon, and as long as any recorded 
lag is constant it can be compensated by delaying all messages by some seconds. </div>
<div style='margin-top:0.25em'>Most of the 1st marathon was formatted with this method.</div>
    </li>
    <p></p>
    <li>The second method uses predetermined anime episode length information, along with break placement patterns, and contextual chat analysis to determine the correct synchronization.
    <div style='margin-top:0.25em'>The placement of breaks in an episode is predicatable, thus the timing of each section of an episode can be predetermined. Break length is unknown and unpredicatable, but can be 
    found using a marathon vod <em>without</em> chat. Even if no vod exists break timing can be estimated and then through detailed analysis of the chat both break timing and episode start time
    can be established, wherein distinguishing chat features provide the needed information.</div>
    <div style='margin-top:0.25em'>The major drawback of this method is the amount of time required to get a high quality synchronization. Also it requires far more effort 
    if things are unpredictable and go wrong, like if the anime freezes and is restarted as occured in the fourth showing of RWBY episode 1. This method works very well when there are repeated showings.</div>
    <div style='margin-top:0.25em'>I used this method a long with personally recorded vods that have twitch's diagnostic information showing stream delay, which makes the RWBY chat logs very high quality.</div>
<br>
</li>
</ol>
<div style='margin-top:0.5em'>Once the timings are correct, they just need to be packaged into the archive files, and loaded by the TCR extension.</div>

</div>
</p>

</div>
