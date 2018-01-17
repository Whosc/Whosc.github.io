---
title: Add A Magento Currency Exchange Update Service
date: 2018-01-16
tags: [magento]
categories:
- 技术
- Magento

---
The default currency exchange rate updater that ships with Magento Community Edition is broken.  


So, we need to replace the default Webservicex with another, working exchange rate updater.

## Custom Module
**File: app/etc/modules/TC_Currency.xml**
<!--more-->
```
<?xml version="1.0"?>
<config>
    <modules>
        <TC_Currency>
            <active>true</active>
            <codePool>local</codePool>
            <depends>
                <Mage_Adminhtml />
                <Mage_Core />
            </depends>
        </TC_Currency>
    </modules>
</config>
```

**File: app/code/local/TC/Currency/etc/config.xml**

```
<?xml version="1.0"?>
<config>
    <modules>
        <TC_Currency>
            <version>1.0.0</version>
        </TC_Currency>
    </modules>
    <global>
        <currency>
            <import>
                <services>
                    <yahoofinance>
                        <name>Yahoo Finance</name>
                        <model>TC_Currency_Model_Yahoo</model>
                    </yahoofinance>
                </services>
            </import>
        </currency>
        <models>
            <tc_currency>
                <class>TC_Currency_Model_Yahoo</class>
            </tc_currency>
        </models>
    </global>
</config>
```

**File: app/code/local/TC/Currency/Model/Yahoo.php**

```
<?php
class TC_Currency_Model_Yahoo extends Mage_Directory_Model_Currency_Import_Abstract
{
    protected $_url = 'http://quote.yahoo.com/d/quotes.csv?s={{CURRENCY_FROM}}{{CURRENCY_TO}}=X&f=l1&e=.csv';
    protected $_messages = array();

    protected function _convert($currencyFrom, $currencyTo, $retry=0) {
        $url = str_replace('{{CURRENCY_FROM}}', $currencyFrom, $this->_url);
        $url = str_replace('{{CURRENCY_TO}}', $currencyTo, $url);

        try {
            sleep(1);
            $handle = fopen($url, "r");
            $exchange_rate = fread($handle, 2000);
            fclose($handle);

            if (!$exchange_rate) {
                $this->_messages[] = Mage::helper('directory')->__('Cannot retrieve rate from %s', $url);
                return null;
            }

            return (float) $exchange_rate * 1.0;
        } catch (Exception $e) {
            if ($retry == 0) {
                $this->_convert($currencyFrom, $currencyTo, 1);
            } else {
                $this->_messages[] = Mage::helper('directory')->__('Cannot retrieve rate from %s', $url);
            }
        }
    }
}

```

By default, the Webservicex model sits at a deeper level than this, we’re just keeping the structure simple.

Our module is finished. You’ll need to flush the caches once this has been uploaded to your Magento installation. The currency update service appears in the following select lists:

* Admin > System > Manage Currency > Rates  
* Admin > System > Configuration > Currency Setup > Scheduled Import Settings > [Service]

**[Note]:** the `http://quote.yahoo.com/d/quotes.csv?s={{CURRENCY_FROM}}{{CURRENCY_TO}}=X&f=l1&e=.csv` is not work now, you need replace an api which can get the data you want !


