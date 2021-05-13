安装
composer
```
$ php composer.phar require yzh52521/apple
或者添加以下代码到composer.json文件的require块中：

"yzh52521/apple": "1.0"
```
//  $orderId 本地订单号
    // $storeProductId 苹果商店产品ID
    // $tradeNo 苹果交易号
    
    $order = findOrder($orderId);

    $applePay = new ApplePay($_POST['receipt'], '');
    if ($applePay->verifyReceipt()) {
        $result = $applePay->query($storeProductId, function ($tradeNo, $returnData) use ($order) {
            // 检查此交易号是否被使用
            if (!$order->checkTradeNoIsUsed($tradeNo)) {
                // 更新本地订单状态等...
                return $this->notify($order, $returnData, $tradeNo);
            } else {
                echo '此笔交易号已经被使用，这笔交易有可能是伪造的！';
                return false;
            }
        });
        if ($result) {
            echo 'success';
        } else {
            echo $applePay->getError();
        }
    } else {
        echo $applePay->getError();
    }
```
