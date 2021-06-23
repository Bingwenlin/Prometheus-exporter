# 阿里云api调用
#!/usr/bin/python3
# -*- coding:utf-8 -*-
# @Date:   2021-05-22 21:01:48
# @Last Modified by:   linwenbin
# @Last Modified time: 2021-04-07
# @Desc: 阿里云账号余额巡检
from aliyunsdkbssopenapi.request.v20171214.QueryAccountBillRequest import QueryAccountBillRequest
from aliyunsdkbssopenapi.request.v20171214.QueryMonthlyBillRequest import QueryMonthlyBillRequest
from aliyunsdkcore import client
from aliyunsdkbssopenapi.request.v20171214 import QueryAccountBalanceRequest
from aliyunsdkcore.profile import region_provider
import json
import requests
import time

def wechatwork(content):
    webhook = ""
    header = {
        "Content-Type": "application/json",
        "Charset": "UTF-8"
    }
    message = {

        "msgtype": "text",
        "text": {
            "content": content
        },
        "at": {

            "isAtAll": True
        }

    }
    message_json = json.dumps(message, ensure_ascii=False)
    message_json = message_json.encode('utf-8')
    info = requests.post(url=webhook, data=message_json, headers=header)
    print(info.text)


# 检查账户余额
def check_account():
    yymm = (time.strftime("%Y-%m", time.localtime()))
    region_provider.add_endpoint('BssOpenApi', 'cn-hangzhou', 'business.aliyuncs.com')
    clt = client.AcsClient('###',**','hangzhou')
    request = QueryAccountBalanceRequest.QueryAccountBalanceRequest()
    request2 = QueryMonthlyBillRequest()
    request.set_accept_format("JSON")
    request2.set_accept_format("JSON")
    request2.set_BillingCycle(yymm)
    result = clt.do_action_with_exception(request)
    result2 = clt.do_action_with_exception(request2)
    res_json = json.loads(str(result, encoding="utf-8"))
    res_json2 = json.loads(str(result2, encoding="utf-8"))

    if res_json["Code"] == "200":
        alyun = res_json["Data"]["AvailableAmount"]
        alyun2 = res_json2["Data"]["NewInvoiceAmount"]
        alyun3 = str(alyun2)
        test = '当前余额：'
        sss = '当月消费：'
        txt3 = '阿里云【至真信息科技】账号余额'
        txt3 = txt3 + ":\n" + test + alyun  + "\n" + sss +alyun3
        return txt3


def check_account2():
    yymm = (time.strftime("%Y-%m", time.localtime()))
    region_provider.add_endpoint('BssOpenApi', 'cn-hangzhou', 'business.aliyuncs.com')
    clt = client.AcsClient('###',**','hangzhou')
    request = QueryAccountBalanceRequest.QueryAccountBalanceRequest()
    request2 = QueryMonthlyBillRequest()
    request.set_accept_format("JSON")
    request2.set_accept_format("JSON")
    request2.set_BillingCycle(yymm)
    result = clt.do_action_with_exception(request)
    result2 = clt.do_action_with_exception(request2)
    res_json = json.loads(str(result, encoding="utf-8"))
    res_json2 = json.loads(str(result2, encoding="utf-8"))

    if res_json["Code"] == "200":
        alyun = res_json["Data"]["AvailableAmount"]
        alyun2 = res_json2["Data"]["NewInvoiceAmount"]
        alyun3 = str(alyun2)
        test = '当前余额：'
        sss = '当月消费：'
        txt3 = '阿里云【gamegoing】账号余额'
        txt3 = txt3 + ":\n" + test + alyun  + "\n" + sss +alyun3
        return txt3


def check_account3():
    yymm = (time.strftime("%Y-%m", time.localtime()))
    region_provider.add_endpoint('BssOpenApi', 'cn-hangzhou', 'business.aliyuncs.com')
    clt = client.AcsClient('###',**','hangzhou')
    request = QueryAccountBalanceRequest.QueryAccountBalanceRequest()
    request2 = QueryMonthlyBillRequest()
    request.set_accept_format("JSON")
    request2.set_accept_format("JSON")
    request2.set_BillingCycle(yymm)
    result = clt.do_action_with_exception(request)
    result2 = clt.do_action_with_exception(request2)
    res_json = json.loads(str(result, encoding="utf-8"))
    res_json2 = json.loads(str(result2, encoding="utf-8"))

    if res_json["Code"] == "200":
        alyun = res_json["Data"]["AvailableAmount"]
        alyun2 = res_json2["Data"]["NewInvoiceAmount"]
        alyun3 = str(alyun2)
        test = '当前余额：'
        sss = '当月消费：'
        txt3 = '阿里云【lefeimobi】账号余额'
        txt3 = txt3 + ":\n" + test + alyun  + "\n" + sss +alyun3
        return txt3

def check_account4():
    yymm = (time.strftime("%Y-%m", time.localtime()))
    region_provider.add_endpoint('BssOpenApi', 'cn-hangzhou', 'business.aliyuncs.com')
    clt = client.AcsClient('###',**','hangzhou')
    request = QueryAccountBalanceRequest.QueryAccountBalanceRequest()
    request2 = QueryMonthlyBillRequest()
    request.set_accept_format("JSON")
    request2.set_accept_format("JSON")
    request2.set_BillingCycle(yymm)
    result = clt.do_action_with_exception(request)
    result2 = clt.do_action_with_exception(request2)
    res_json = json.loads(str(result, encoding="utf-8"))
    res_json2 = json.loads(str(result2, encoding="utf-8"))

    if res_json["Code"] == "200":
        alyun = res_json["Data"]["AvailableAmount"]
        alyun2 = res_json2["Data"]["NewInvoiceAmount"]
        alyun3 = str(alyun2)
        test = '当前余额：'
        sss = '当月消费：'
        txt3 = '阿里云【zwqing333@163.com】账号余额'
        txt3 = txt3 + ":\n" + test + alyun  + "\n" + sss +alyun3
        return txt3

if __name__ == '__main__':
    wechatwork(check_account())
    wechatwork(check_account2())
    wechatwork(check_account3())
    wechatwork(check_account4())



