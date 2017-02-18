# bitcoin-payment-gateway
Free Bitcoin Payment Gateway from FarhadExchange.net
درگاه مستقیم بیت کوین روی بلاک چین فرهاد اکسچنج – کاملا رایگان
درگاه مستقیم بیت کوین روی بلاک چین فرهاد اکسچنج – کاملا رایگان

هر کاربر سایت فرهاد اکسچنج دات نت – https://farhadexchange.net – این امکان را دارد که دارای یک درگاه مستقیم بیت کوین روی سایت یا وبلاگ خود باشد و به راحتی از همه جای دنیا پول به صورت بیت کوین دریافت کند. بیت کوین دریافت شده را می توانید به صورت بیت کوین و اتوماتیک و یا ریال برداشت کنید. 
درگاه مستقیم بیت کوین فرهاد اکسچنج بدون کارمزد و کاملا رایگان است. شرکت فرهاد اکسچنج تضمین می کند که این سرویس روی بلاک چین اختصاصی فرهاد اکسچنج برای مدت نامحدود رایگان خواهد ماند. 
 
 ورود به مطلب
 
با استفاده از API فرهاد اکسچنج به راحتی قادر خواهید بود که ظرف مدت چند دقیقه روی سایت یا وبلاگ خود یک درگاه کاملا اتوماتیک دریافت بیت کوین ایجاد کنید. 
یکی از مشکلات اصلی ایجاد درگاههای بیت کوین آدرس های منحصر به فرد برای هر حساب است که گروه برنامه نویسی فرهاد اکسچنج این مهم را آسان کرده است. شما از طریق API فرهاد اکسچنج قادر به ایجاد بی نهایت آدرس منحصر به فرد بیت کوین خواهید بود که موجودی هر کدام از آنها به طور اتوماتیک نزد حساب اصلی شما روی بلاک چین فرهاد اکسچنج فوروارد می شود. 
کلیه ی اطلاعات وجه دریافتی از مشتری شما به صورت یک callback اتوماتیک از سرور ما به سایت شما مخابره می شود و کلیه ی عملیات به طور کاملا امن و اتوماتیک راهبری می شوند. 
 
 دریافت xkey , xpub
برای استفاده از API فرهاد اکسچنج و داشتن درگاه مستقیم بیت کوین روی سایت خود احتیاج به دو کلید xkey و xpub دارید. در قسمت پشتیبانی فرهاد اکسچنج درخواست دهید تا این دو کلید ظرف ۲۴ ساعت برای شما تولید و ارسال شود. با داشتن این دو کلید به طور کامل درگاه مستقیم شما آماده خواهد بود. 
 
 تولید آدرس منحصر به فرد بیت کوین
 
هر مشتری شما برای هر مبلغی که بخواهد به حساب شما واریز کند احتیاج به یک آدرس منحصر به فرد بیت کوین خواهد داشت. شما با فراخواندن آدرس زیر و با داشتن دو کلید xpub و xkey می توانید برای مشتری خود به طور اتوماتیک یک آدرس منحصر به فرد ایجاد کنید:
 
https://farhadexchange.net/bitcoin-generate-address-api.php?xpub={xpub}&xkey={xkey}&callback_url={callback_url}
در پاسخ به متد فوق یک آدرس منحصر به فرد به صورت JSON ایجاد می شود:
{“address”:“19jJyiC6DnKyKvPg38eBE8R6yCSXLLEjqw”,“index”:23,“callback”:“https://mystore.com?invoice_id=058921123”}
که مشتری شما باید مبلغ بیت کوین را به این آدرس ایجاد شده بفرستد.
نمونه کد پی اچ پی برای ایجاد آدرس منحصر بفرد بیت کوین
$secret = ‘ZzsMLGKe162CfA5EcG6j’;
$my_xpub = ‘{YOUR XPUB ADDRESS}’;
$my_api_key = ‘{YOUR API KEY}’;
$my_callback_url = ‘https://mystore.com?invoice_id=058921123&secret=’.$secret;
$root_url = ‘https://farhadexchange.net/bitcoin-generate-address-api.php’;
$parameters = ‘xpub=’ .$my_xpub. ‘&callback=’ .urlencode($my_callback_url). ‘&xkey=’ .$my_api_key;
$response = file_get_contents($root_url . ‘?’ . $parameters);
$object = json_decode($response);
echo ‘Send Payment To : ‘ . $object->address;
 مشخصات کال بک
چنانچه مشتری شما مبلغ را به آدرس بیت کوین مورد نظر واریز کند یک کال بک ایجاد می شود که اطلاعات واریز را به سایت یا وبلاگ شما منتقل می کند. این اطلاعات شامل موارد زیر است:
transaction_hash – The payment transaction hash.
address – The destination bitcoin address (part of your xPub account).
confirmations – The number of confirmations of this transaction.
value – The value of the payment received (in satoshi, so divide by 100,000,000 to get the value in BTC).
 
 مثال PHP  از نحوه ی دریافت کال بک. اطلاعات کال بک با متد GET ارسال می شود:
 
$real_secret = 'ZzsMLGKe162CfA5EcG6j';
$invoice_id = $_GET['invoice_id']; //invoice_id is passed back to the callback URL
$transaction_hash = $_GET['transaction_hash'];
$value_in_satoshi = $_GET['value'];
$value_in_btc = $value_in_satoshi / 100000000;

//Commented out to test, uncomment when live
if ($_GET['test'] == true) {
    return;
}

try {
  //create or open the database
  $database = new SQLiteDatabase('db.sqlite', 0666, $error);
} catch(Exception $e) {
  die($error);
}

//Add the invoice to the database
$stmt = $db->prepare("replace INTO invoice_payments (invoice_id, transaction_hash, value) values(?, ?, ?)");
$stmt->bind_param("isd", $invoice_id, $transaction_hash, $value_in_btc);

if($stmt->execute()) {
   echo "*ok*";
}
