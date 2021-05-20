# Powerpage Markdown Editor

``Powerpage Markdown Editor`` is a markdown editor using [**Powerpage**](https://github.com/casualwriter/powerpage) with 
 js library of [*simplemde-markdown-editor*](https://github.com/sparksuite/simplemde-markdown-editor). 
 
 It is a simple html/js application demonstrating developing application using [Powerpage](https://github.com/casualwriter/powerpage).

![Powerpage Markdown Editor](powerpage-md.jpg)


## Installation & Run

* No installation is needed, Just download and run ``powerpage.exe``.
* The package is same as [Powerpage](https://github.com/casualwriter/powerpage), only ``powerpage.ini`` is revised.


## Source Code

It is single html/js program ([markdown.html](source/markdown.html)) in 80 lines, showing in below code-block. 

```
<!DOCTYPE html>
<html>
<title>Powerpage Markdown Editor</title>
<link rel="stylesheet" href="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.css">
<script src="https://cdn.jsdelivr.net/simplemde/latest/simplemde.min.js"></script>
<style>
.CodeMirror {	height:calc(100vh - 160px); }
</style>

<body style="font-family:calibri">
  <div id=header style="padding:8px">
    <span style="font-size:20px;font-weight:700;">Markdown Editor </span>
    <a target=_new href="https://github.com/sparksuite/simplemde-markdown-editor">(SimpleMDE)</a>&nbsp;&nbsp;&nbsp;&nbsp;
    <b>File: </b><span id=filename style="color:green">new file</span>  
    <span style="float:right">
    <button onclick="window.location='powerpage.html'">PowerPage</button>&nbsp;&nbsp;&nbsp;
    <button onclick="pb.callback('onOpenFile').file.select('Markdown (*.md),*md')" accesskey=o><b>O</b>pen</button>
    <button onclick="onPrint()" accesskey=p disabled><b>P</b>rint</button>&nbsp;&nbsp;&nbsp;
    <button onclick="onSave()" accesskey=s><b>S</b>ave</button>
    <button onclick="pb.callback('onSaveAs').file.select('Markdown (*.md),*md')" assesskey=a>Save <b>A</b>s</button>
    </span> 
  </div>
  <textarea id="content"></textarea>
</body>

<script>
// initial simple MDE (https://github.com/sparksuite/simplemde-markdown-editor)
var simplemde = new SimpleMDE( { element: document.getElementById("content") } );
var mdFile = document.getElementById("filename").innerText
var mdText = ''

// open file
function onOpenFile(result, type, url) {
  var rs = JSON.parse( result.split('\\').join('\\\\') )
  if (rs.status>0) {
    pb.callback('onLoadFile').file.read(rs.path)
    document.getElementById("filename").innerText = mdFile = (rs.path||mdFile)
    pb.microhelp( 'File ' + mdFile + ' loaded.' )
  }   
}

// load file
function onLoadFile(result, type, url) {
  simplemde.value( mdText = result )
}

// save file
function onSave() {
  if (mdFile == 'new file') {
    pb.callback('onSaveAs').file.saveas('Markdown (*.md),*md')
  } else if ( simplemde.value() == mdText ) {
    alert('Data no change. no need to save!')
  } else {
    mdText = simplemde.value()
    pb.file.write( mdFile, '@mdText', 'onAfterSave' )
  }  
}

function onAfterSave(result, type, url) {
  alert('File has been saved to ' + mdFile + '!')
}

// saveas
function onSaveAs(result, type, url) {
  var rs = JSON.parse( result.split('\\').join('\\\\') )
  if (rs.status>0) {
    mdFile = (rs.path.substr(-3)=='.md') ? rs.path : rs.path+'.md'
    document.getElementById("filename").innerText = mdFile 
    onSave()
  }  
}

// prompt save data when close
function onPageClose() {
  if ( simplemde.value() !== mdText && mdFile !== 'new file' && confirm('Data has been change. Save Data?')) {
    mdText = simplemde.value()
    pb.file.write( mdFile, '@mdText', 'close' )
    return 'no'
  }  
}
</script>
```


## Modification History

* 2021/05/14, first version with powerpage v0.43
* 2021/05/20, using powerpage v0.45


## License

MIT

