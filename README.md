import requests

def check_wallet():
    print("=== د کریپټو والېټ چیکر ===")
    wallet = input("والېټ اډریس دننه کړئ: ")
    blockchain = input("بلاکچین (xrp/doge/usdt/aave): ").lower()
    
    apis = {
        "xrp": f"https://data.xrplf.org/v1/account/{wallet}",
        "doge": f"https://dogechain.info/api/v1/address/balance/{wallet}",
        "usdt": f"https://api.etherscan.io/api?module=account&action=tokentx&address={wallet}&contractaddress=0xdac17f958d2ee523a2206206994597c13d831ec7",
        "aave": f"https://api.etherscan.io/api?module=account&action=tokentx&address={wallet}&contractaddress=0x7fc66500c84a76ad7e9c93437bfc5ac33e2ddae9"
    }
    
    try:
        res = requests.get(apis[blockchain])
        data = res.json()
        
        if blockchain == "xrp":
            print(f"✅ XRP والېټ فعالیت: {len(data.get('transactions', []))} ټرانزاکشنونه")
        elif blockchain == "doge":
            print(f"✅ DOGE بیلانس: {data['balance']} DOGE" if data['success'] == 1 else "❌ والېټ نشته!")
        elif blockchain in ["usdt", "aave"]:
            print(f"✅ {blockchain.upper()} فعالیتونه: {len(data['result'])}" if data['result'] else "❌ هیڅ فعالیت نشته!")
    except:
        print("❌ تېروتنه: بلاکچین API ته لاسرسی نشته!")

check_wallet()