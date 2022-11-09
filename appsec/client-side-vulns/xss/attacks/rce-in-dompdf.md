# RCE in DOMPDF

Если в конфигурации DomPDF включена опция `DOMPDF_ENABLE_PHP`, то при внедрении такого кода `<script type="text/php">php_code</script>` он будет исполнен.
