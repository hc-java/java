~~~java
//原名：html2pdf.bundle.js
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>Bootstrap 实例</title>
    <!-- 包含头部信息用于适应不同设备 -->
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>

<body>

<p id="element-to-print">hello-这是下载的内容呀！</p>
<script type="text/javascript" src="js/pdf.js">
    // var element = document.getElementById('element-to-print');
    // html2pdf(element);
</script>
<script type="text/javascript">
    var element = "<!DOCTYPE html><p style='color:red'>Hello World</p></html>";
    var opt = {
        margin:       1,
        filename:     'myfile.pdf',
        image:        { type: 'jpeg', quality: 0.98 },
        html2canvas:  { scale: 2 },
        jsPDF:        { unit: 'in', format: 'letter', orientation: 'portrait' }
    };

    // New Promise-based usage:
    html2pdf().set(opt).from(element).save();
</script>

</body>

</html>
~~~

