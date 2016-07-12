---
layout: docwithnav
---

<!-- BEGIN: Gotta keep this section JS/HTML because it swaps out content dynamically -->
<p>&nbsp;</p>
<script language="JavaScript">
var forwarding=window.location.hash.replace("#","");
$( document ).ready(function() {
    if(forwarding) {
    	$("#generalInstructions").hide();
    	$("#continueEdit").show();
    	$("#continueEditButton").text("Edit " + forwarding);
    	$("#continueEditButton").attr("href", "https://github.com/starboychina/kubernetes.kansea.com/edit/gh-pages/" + forwarding)
    } else {
        $("#generalInstructions").show();
    	$("#continueEdit").hide();
    }
});
</script>
<div id="continueEdit">

<h2>继续编辑</h2>

<p>点击下面的链接来编辑刚才的页面。编辑完成以后，点击屏幕下方的"Commit Changes"。它会在你的 Github 上创建一个 "fork"。创建完成以后，你还可以在你的"fork"上修改其他页面。如果你要把修改后的页面发送给我们，你只需要到你的“fork”页面，然后点击"New Pull Request"。</p>

<p><a id="continueEditButton" class="button"></a></p>

</div>
<div id="generalInstructions">

<h2>在云端编辑我们的网站</h2>

<p>点击下面的按钮来访问我们网站的代码库，然后你可以点击右上角的“Fork”按钮，来复制我们的网站到你的 Github 上一个“fork”。你可以在你的的“fork”上修改一些页面，修改完毕以后，你可以在“fork”的主页点击 "New Pull Request" 来吧你的修改发送给我们。</p>

<p><a class="button" href="https://github.com/starboychina/kubernetes.kansea.com/">浏览网站源代码</a></p>

</div>
<!-- END: Dynamic section -->


{% include_relative README.md %}
