# amsystem
Attendence Management System using Python


-------------------------------------------------------------------------------------------------------------------------------------


import pymysql
import pandas as pd
import time
def menu():
    ch = 0
    while(ch<6 or ch>0):
        print("M A I N  M E N U...")
        print(" ~~~~~~~~~~~~~~~~~~~ ")
        print("1 >New employee registration")
        print("2 >log in")
        print("3 >log out")
        print("4 > ADMINISTRATIVE log in")
        print("5 > E X I T...")
        print(" ~~~~~~~~~~~~~~~~~~~ ")
    
        ch = int(input("Please enter your choice number -> "))
        return ch
    
choice = 0
while (choice != 5):
    
    choice = menu()
    if choice==1:
        rp = int(input("Please enter password to register given by your company :"))
        if rp == 1234:
            try:
                print("welcome to all the new Employees")
                db = pymysql.connect("localhost","root","","attendance_management" )
                cursor = db.cursor()
                id1=int(input("Enter the Id No. :"))
                name=input("Enter Name :")
                logid=input("Enter Username :")
                pass1=input("enter password :")
                address=input("Enter Address :")
                mob=int(input("Enter Mobile No :"))
                ab=cursor.execute("insert into employee_list values(%s,%s,%s,%s,%s,%s)",
                                        (id1,name,logid,pass1,address,mob) )
                db.commit()
                print("Registered Successfully")
                db.close()
            except:
                print("Registration failed")
                choice=menu()
        else:
            print("You entered WRONG PASSWORD enter correct password to register :")
            
    elif choice==2:
        print("welcome to login portal")
        db1 = pymysql.connect("localhost","root","","attendance_management" )
        cursor = db1.cursor()
        id1=input("Enter the Log in Id :")
        name1=input("Enter Password :")
        sql="SELECT * FROM employee_list WHERE login_id='%s' AND password='%s'" % (id1,name1)
        cursor.execute(sql)
        data=cursor.fetchall()
        
        if(len(data)==0):
            print("Wrong Username or password .....!!!!!    ")
            continue
        date1=time.localtime()
        date2=0
        cursor.execute("insert into attendance_list values(%s,%s,%s,%s)",
                                 (data[0][0],data[0][1],date1,date2) )
        db1.commit()
        db1.close()
        print("you are successfully logged in")
        
    elif choice==3:
        print("welcome to logout portal")
        db2 = pymysql.connect("localhost","root","","attendance_management" )
        cursor = db2.cursor()
        id1=input("Enter the login Id :")
        name2=input("Enter Password :")
        sql="SELECT * FROM employee_list WHERE login_id='%s' AND password='%s'" % (id1,name2)
        cursor.execute(sql)
        data=cursor.fetchall()
        
        if(len(data)==0):
            print("Wrong Username or password .....!!!!!    ")
            continue
        date2=time.localtime()
        cursor.execute("UPDATE attendance_list SET log_out_time = NOW()") 
        #WHERE login_id = '%s'" % (id1))
        db2.commit()
        db2.close()
        print("you are successfully logged out")
    elif choice==4:
        cdbp = int(input("Enter Database password given by Authority :"))
        dbp = 123
        if cdbp == dbp:
            print("welcome admin to login portal")
            db = pymysql.connect("localhost","root","","attendance_management" )
            cursor = db.cursor()
            db.commit()
            db.close()
            def sub_menu():
                ch2 = 0
                while(ch2<10 or ch2>5):
                    print("M A I N  M E N U...")
                    print("~~~~~~~~~~~~~~~~~~~")
                    print("6 >Admin check the worksheet daily")
                    print("7 >Admin late entry")
                    print("8 >Admin early leave")
                    print("9 > Go to main menu...")
                    print("~~~~~~~~~~~~~~~~~~~")
                    ch2 = int(input("Please enter your choice number -> "))
                    return ch2
            choice = 0
            while (choice != 9):
                choice = sub_menu()
                if choice==6:
                    print("welcome admin to check the daily attendance of all the employees")
                    db = pymysql.connect("localhost","root","","attendance_management" )
                    cursor = db.cursor()
                    sql = "SELECT * FROM attendance_list" 
                    try:
                        cursor.execute(sql)
                        results = cursor.fetchall()
                        columns = ['Emp Id','emp name','logintime','logouttime']
                        coll=pd.DataFrame([['','','','']],columns=columns)
                        for row in results:
                            emp_id = row[0]
                            emp_name = row[1]
                            log_in_time=row[2]
                            log_out_time=row[3]
                            coll = coll.append(pd.DataFrame([[ emp_id, emp_name, log_in_time, log_out_time]],
                                                            columns=columns))
                        print(coll)
                    except:
                            print ("Error: unable to fetch data")
                            db.close()
                            
                elif choice==7:
                    print("welcome admin in late entry checking portal :")
                    db = pymysql.connect("localhost","root","","attendance_management" )
                    cursor = db.cursor()
                    sql="SELECT * FROM attendance_list "
                    cursor.execute(sql)
                    data = cursor.fetchall()
                    columns = ['Emp Id','Emp Name','logintime','logouttime']
                    coll=pd.DataFrame([['','','','']],columns=columns)
                    for row in data:
                            h1 = row[3].hour
                            m1 = row[3].minute
                            if(h1>=10 and m1>=0):
                                coll = coll.append(pd.DataFrame([[ row[0], row[1], row[2], row[3]]],
                                                                columns=columns))
                    print("Late entry Employees are:-")
                    print(coll)
                    db.close()
                    
                elif choice==8:
                    print("welcome admin in early leave checking portal :")
                    db = pymysql.connect("localhost","root","","attendance_management" )
                    cursor = db.cursor()
                    sql="SELECT * FROM attendance_list "
                    cursor.execute(sql)
                    data = cursor.fetchall()
                    columns = ['Emp Id','Emp Name','logintime','logouttime']
                    coll=pd.DataFrame([['','','','']],columns=columns)
                    for row in data:
                            h2 = row[2].hour
                            m2 = row[2].minute
                            if(h2<=18 and m2>=0):
                                coll = coll.append(pd.DataFrame([[ row[0], row[1], row[2], row[3]]], columns=columns))
                    print("Early leave Employees are:-")
                    print(coll)
                    db.close()
                    
                elif choice==9:
                    print("Accessing main menu......")
                    choice = menu()
                else:
                    print ("WRONG CHOICE ! Please enter your choice between 6 and 9")
        else:
                    print("You are not authorized to access")
    
    elif choice==5:
                    print("exit......")
    else:
                    print ("WRONG CHOICE ! Please enter your choice between 1 and 5")
                    
                    
--------------------------------------------------------------------------------------------------------------------------------------



