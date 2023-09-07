import mysql.connector as mycon
from datetime import datetime

today = datetime.today()

rdb = mycon.connect(
    host="localhost",
    user="root",
    password="root",
    port="3306",
    database="Restaurant")

rcur = rdb.cursor()
rcur.execute("SELECT * FROM ItemMaster")
ItemMaster = rcur.fetchall()

icur = rdb.cursor()
itemid = 0
bill = 0.0


def select(itemid, bill):
    for data in ItemMaster:
        print(data)
    order = input("Enter Valid order code: ")

    while order != "0":
        itemid += 1
        quantity = int(input("Enter Quantity: "))
        icur.execute("SELECT ItemCode, ItemCost FROM ItemMaster WHERE ItemCode = '" + order + "'")

        icost = icur.fetchone()
        cost = icost[1]
        print("Price: ", cost)
        total = cost * float(quantity)
        print("Total: ", total)
        gst = total * (12 / 100)
        print("GST Amount-(12%): ", gst)
        amount = total + gst
        print("Total Amount Inclusive GST: ", amount)

        sql = "INSERT INTO TransactionDetails (TransID, TransDateTime, ItemCode, ItemQuantity, ItemCost, ItemTotal, " \
              "GSTAmount, TotalAmount)" \
              "VALUES (%s, %s, %s, %s, %s, %s, %s, %s)"
        val = (itemid, today, order, quantity, cost, total, gst, amount)
        icur.execute(sql, val)
        rdb.commit()

    order = input("Wanna order more (y/n): ").lower()
    bill += amount

    if order == "y":
        select(itemid, bill)
    else:
        print("Thanks for your order."
              "\nBill Generated: ", bill)
        icur.execute("SELECT * FROM TransactionDetails")
        td = icur.fetchall()
        for d2 in td:
            print(d2)
        exit()


select(itemid, bill)
