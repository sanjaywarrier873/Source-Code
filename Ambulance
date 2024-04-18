from flask import Flask
from flask import Flask, render_template, Response, redirect, request, session, abort, url_for
import base64
from datetime import datetime
from datetime import date
import datetime
import random
from random import seed
from random import randint
import math
import sys
import rsa
import cv2
import csv
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.image as img
import threading
import os
import time
import shutil
import imagehash
import hashlib
import PIL.Image
from PIL import Image
from PIL import ImageTk
import urllib.request
import urllib.parse
from urllib.request import urlopen
import webbrowser
import mysql.connector

mydb = mysql.connector.connect(
  host="localhost",
  user="root",
  passwd="",
  charset="utf8",
  database="ambulance_positioning"
)


app = Flask(__name__)
##session key
app.secret_key = 'abcdef'
@app.route('/',methods=['POST','GET'])
def index():
    cnt=0
    act=""
    msg=""
    ff=open("det.txt","w")
    ff.write("1")
    ff.close()

    ff1=open("photo.txt","w")
    ff1.write("1")
    ff1.close()

    ff11=open("img.txt","w")
    ff11.write("1")
    ff11.close()

    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    ff11=open("title1.txt","r")
    title1=ff11.read()
    ff11.close()

    ff11=open("title2.txt","r")
    title2=ff11.read()
    ff11.close()
    
        

    return render_template('index.html',msg=msg,act=act,title=title,title1=title1,title2=title2)

@app.route('/login', methods=['POST','GET'])
def login():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    ff11=open("title1.txt","r")
    title1=ff11.read()
    ff11.close()

    ff11=open("title2.txt","r")
    title2=ff11.read()
    ff11.close()
    
    if request.method == 'POST':
        uname = request.form['uname']
        pass1 = request.form['pass']
        mycursor = mydb.cursor()
        mycursor.execute("SELECT count(*) FROM ap_admin where username=%s && password=%s && utype='2'",(uname,pass1))
        myresult = mycursor.fetchone()[0]
        if myresult>0:
            
            session['username'] = uname
            
            return redirect(url_for('nhai_home')) 
        else:
            msg="Incorrect username/password!!!"
                
    
    return render_template('login.html',msg=msg,title=title,title1=title1,title2=title2)

@app.route('/login_user', methods=['POST','GET'])
def login_user():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    ff11=open("title1.txt","r")
    title1=ff11.read()
    ff11.close()

    ff11=open("title2.txt","r")
    title2=ff11.read()
    ff11.close()
    
    if request.method == 'POST':
        uname = request.form['uname']
        pass1 = request.form['pass']
        mycursor = mydb.cursor()
        mycursor.execute("SELECT count(*) FROM ap_register where uname=%s && pass=%s",(uname,pass1))
        myresult = mycursor.fetchone()[0]
        if myresult>0:
            
            session['username'] = uname
            
            return redirect(url_for('userhome')) 
        else:
            msg="Incorrect username/password!!!"
                
    
    return render_template('login_user.html',msg=msg,title=title,title1=title1,title2=title2)

@app.route('/login_admin', methods=['POST','GET'])
def login_admin():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    ff11=open("title1.txt","r")
    title1=ff11.read()
    ff11.close()

    ff11=open("title2.txt","r")
    title2=ff11.read()
    ff11.close()
    
    
    if request.method == 'POST':
        uname = request.form['uname']
        pass1 = request.form['pass']
        mycursor = mydb.cursor()
        mycursor.execute("SELECT count(*) FROM ap_admin where username=%s && password=%s && utype='1'",(uname,pass1))
        myresult = mycursor.fetchone()[0]
        if myresult>0:
            result=" Your Logged in sucessfully**"
            session['username'] = uname
            
            return redirect(url_for('register')) 
        else:
            msg="You are logged in fail!!!"
                
    
    return render_template('login_admin.html',msg=msg,title=title,title1=title1,title2=title2)

@app.route('/login2', methods=['POST','GET'])
def login2():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    ff11=open("title1.txt","r")
    title1=ff11.read()
    ff11.close()

    ff11=open("title2.txt","r")
    title2=ff11.read()
    ff11.close()
    
    if request.method == 'POST':
        uname = request.form['uname']
        pass1 = request.form['pass']
        mycursor = mydb.cursor()
        mycursor.execute("SELECT count(*) FROM ap_traffic where uname=%s && pass=%s",(uname,pass1))
        myresult = mycursor.fetchone()[0]
        if myresult>0:
            result=" Your Logged in sucessfully**"
            session['username'] = uname
            ff1=open("log.txt","w")
            ff1.write(uname)
            ff1.close()
            return redirect(url_for('admin_control')) 
        else:
            msg="You are logged in fail!!!"
                
    
    return render_template('login2.html',msg=msg,title=title,title1=title1,title2=title2)


@app.route('/nhai_home',methods=['POST','GET'])
def nhai_home():
    msg=""
    act=request.args.get("act")
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    #if 'username' in session:
    #    uname = session['username']
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_ambulance")
    data = mycursor.fetchall()

    if request.method=='POST':
        ambulance_no=request.form['ambulance_no']
        area=request.form['area']
        city=request.form['city']
        driver=request.form['driver']
        mobile=request.form['mobile']

        mycursor.execute("SELECT count(*) FROM ap_ambulance where ambulance_no=%s",(ambulance_no, ))
        cnt = mycursor.fetchone()[0]
        if cnt==0:
            
            mycursor.execute("SELECT max(id)+1 FROM ap_ambulance")
            maxid = mycursor.fetchone()[0]
            if maxid is None:
                maxid=1

            sql = "INSERT INTO ap_ambulance(id,ambulance_no,area,city,driver,mobile) VALUES (%s, %s, %s, %s,%s,%s)"
            val = (maxid,ambulance_no,area,city,driver,mobile)
            
            mycursor.execute(sql, val)
            mydb.commit()
            msg="success"
        else:
            msg="fail"
        
    if act=="del":
        did=request.args.get("did")
        mycursor.execute("delete from ap_ambulance where id=%s",(did,))
        mydb.commit()
        return redirect(url_for('nhai_home'))
    

    return render_template('nhai_home.html',msg=msg,data=data,title=title)

@app.route('/edit_ambulance',methods=['POST','GET'])
def edit_ambulance():
    msg=""
    aid=request.args.get("aid")
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    #if 'username' in session:
    #    uname = session['username']
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_ambulance where id=%s",(aid,))
    data1 = mycursor.fetchone()
    
    mycursor.execute("SELECT * FROM ap_ambulance")
    data = mycursor.fetchall()

    if request.method=='POST':
        ambulance_no=request.form['ambulance_no']
        area=request.form['area']
        city=request.form['city']
        driver=request.form['driver']
        mobile=request.form['mobile']

        mycursor.execute("update ap_ambulance set ambulance_no=%s,area=%s,city=%s,driver=%s,mobile=%s where id=%s",(ambulance_no,area,city,driver,mobile,aid))
        mydb.commit()
        msg="success"
       
       
    return render_template('edit_ambulance.html',msg=msg,data=data,title=title,data1=data1)


@app.route('/add',methods=['POST','GET'])
def add():
    result=""
    msg=""
    name=""
    email=""
    mess=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()
    
    act=request.args.get("act")
    
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_traffic")
    data = mycursor.fetchall()

    mycursor.execute("SELECT max(id)+1 FROM ap_traffic")
    maxid = mycursor.fetchone()[0]
    if maxid is None:
        maxid=1
    tid="TC"+str(maxid)
        
    
    if request.method=='POST':
        
        name=request.form['name']
        uname=request.form['uname']
        location=request.form['location']
        city=request.form['city']
        pass1=request.form['pass']
        mobile=request.form['mobile']
        email=request.form['email']
        
        now = datetime.datetime.now()
        rdate=now.strftime("%d-%m-%Y")
        
        
        
        mycursor.execute("SELECT count(*) FROM ap_traffic where uname=%s",(uname, ))
        cnt = mycursor.fetchone()[0]
        if cnt==0:
            
            sql = "INSERT INTO ap_traffic(id, name,location,city,uname,pass,mobile,email) VALUES (%s, %s, %s, %s, %s, %s,%s,%s)"
            val = (maxid,name,location,city,uname,pass1,mobile,email)
            print(sql)
            mycursor.execute(sql, val)
            mydb.commit()

            mess="Dear "+name+", Traffic Police ID: "+uname+", Password: "+pass1
            msg="success"
        else:
            msg="fail"
            
    if act=="del":
        did=request.args.get("did")
        mycursor.execute("delete FROM ap_traffic where id=%s",(did,))
        mydb.commit()
        return redirect(url_for('add'))
        
    return render_template('add.html',msg=msg,data=data,act=act,tid=tid,mess=mess,email=email,name=name,title=title)

@app.route('/edit_traffic',methods=['POST','GET'])
def edit_traffic():
    result=""
    aid=request.args.get("aid")
    msg=""
    name=""
    email=""
    mess=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()
    
    act=request.args.get("act")
    
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_traffic where id=%s",(aid,))
    data1 = mycursor.fetchone()
    
    mycursor.execute("SELECT * FROM ap_traffic")
    data = mycursor.fetchall()

    mycursor.execute("SELECT max(id)+1 FROM ap_traffic")
    maxid = mycursor.fetchone()[0]
    if maxid is None:
        maxid=1
    tid="TC"+str(maxid)
        
    
    if request.method=='POST':
        
        name=request.form['name']
       
        location=request.form['location']
        city=request.form['city']
        pass1=request.form['pass']
        mobile=request.form['mobile']
        email=request.form['email']

        mycursor.execute("update ap_traffic set name=%s,location=%s,city=%s,mobile=%s,email=%s,pass=%s where id=%s",(name,location,city,mobile,email,pass1,aid))
        mydb.commit()
        msg="success"
        
      
        
    return render_template('edit_traffic.html',msg=msg,data=data,act=act,tid=tid,mess=mess,email=email,name=name,title=title,data1=data1)


@app.route('/add_hospital',methods=['POST','GET'])
def add_hospital():
    msg=""
    act=request.args.get("act")
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    #if 'username' in session:
    #    uname = session['username']
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_hospital")
    data = mycursor.fetchall()

    if request.method=='POST':
        hospital=request.form['hospital']
        area=request.form['area']
        city=request.form['city']
        mobile=request.form['mobile']
            
        mycursor.execute("SELECT max(id)+1 FROM ap_hospital")
        maxid = mycursor.fetchone()[0]
        if maxid is None:
            maxid=1

        sql = "INSERT INTO ap_hospital(id,hospital,area,city,mobile) VALUES (%s, %s, %s, %s,%s)"
        val = (maxid,hospital,area,city,mobile)
        
        mycursor.execute(sql, val)
        mydb.commit()
        msg="success"

    if act=="del":
        did=request.args.get("did")
        mycursor.execute("delete from ap_hospital where id=%s",(did,))
        mydb.commit()
        return redirect(url_for('add_hospital'))
        

    return render_template('add_hospital.html',msg=msg,data=data,title=title)

@app.route('/edit_hospital',methods=['POST','GET'])
def edit_hospital():
    msg=""
    hid=request.args.get("hid")
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    #if 'username' in session:
    #    uname = session['username']
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_hospital where id=%s",(hid,))
    data1 = mycursor.fetchone()
    
    mycursor.execute("SELECT * FROM ap_hospital")
    data = mycursor.fetchall()

    if request.method=='POST':
        hospital=request.form['hospital']
        area=request.form['area']
        city=request.form['city']
        mobile=request.form['mobile']
            
        mycursor.execute("update ap_hospital set hospital=%s,area=%s,city=%s,mobile=%s where id=%s",(hospital,area,city,mobile,hid))
        mydb.commit()
        msg="success"
      

    return render_template('edit_hospital.html',msg=msg,data=data,title=title,data1=data1)

@app.route('/upload',methods=['POST','GET'])
def upload():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    #if 'username' in session:
    #    uname = session['username']
    if request.method == 'POST':
        file = request.files['file']
        fnn="datafile.csv"
        file.save(os.path.join("static/upload", fnn))
        msg="success"

    return render_template('upload.html',msg=msg,title=title)

@app.route('/optimize_data',methods=['POST','GET'])
def optimize_data():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
   
    return render_template('optimize_data.html',msg=msg,title=title)


@app.route('/admin_control', methods=['POST','GET'])
def admin_control():

    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()
    
    mycursor = mydb.cursor()
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    data=[]
    st=""

    mycursor.execute("SELECT distinct(city) FROM ap_alert")
    data1 = mycursor.fetchall()

    if request.method == 'POST':
        city = request.form['city']
        mycursor.execute("SELECT count(*) FROM ap_alert where city=%s",(city,))
        dd = mycursor.fetchone()[0]
        if dd>0:
            st="1"
            mycursor.execute("SELECT * FROM ap_alert where city=%s",(city,))
            data = mycursor.fetchall()
            
    return render_template('admin_control.html',data1=data1,data=data,st=st,title=title)


@app.route('/vehicle', methods=['POST','GET'])
def vehicle():
    msg=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    ff11=open("title1.txt","r")
    title1=ff11.read()
    ff11.close()

    ff11=open("title2.txt","r")
    title2=ff11.read()
    ff11.close()

    ff=open("static/amno.txt","w")
    ff.write("")
    ff.close()
    
    
    if request.method == 'POST':
        vno = request.form['vno']

        mycursor = mydb.cursor()

        utype='admin'
        mycursor.execute("SELECT count(*) FROM ap_register where vno=%s",(vno,))
        myresult = mycursor.fetchone()[0]
        if myresult>0:
            mycursor.execute("SELECT id FROM ap_register where vno=%s",(vno,))
            vid = mycursor.fetchone()[0]
            ff1=open("un.txt","w")
            ff1.write(vno)
            ff1.close()
            result=" Your Logged in sucessfully**"
            return redirect(url_for('vehicle_move',vid=vid)) 
        else:
            result="You are logged in fail!!!"

    
    
    return render_template('vehicle.html',msg=msg,title=title,title1=title1,title2=title2)

'''@app.route('/login2', methods=['POST','GET'])
def login2():
    result=""
    ff1=open("photo.txt","w")
    ff1.write("1")
    ff1.close()
    data2=[]
    mycursor = mydb.cursor()
    mycursor.execute("SELECT distinct(city) FROM fv_police")
    data1 = mycursor.fetchall()
        
    
    if request.method == 'POST':
        city = request.form['city']
        mycursor.execute("SELECT distinct(area) FROM fv_police where city=%s",(city,))
        data2 = mycursor.fetchall()
        
        area = request.form['area']
        
        mycursor.execute("SELECT count(*) FROM fv_police where city=%s && area=%s",(city,area))
        myresult = mycursor.fetchone()[0]
        if myresult>0:
            result=" Your Logged in sucessfully**"
            #session['username'] = uname
            ff1=open("log.txt","w")
            ff1.write(uname)
            ff1.close()
            return redirect(url_for('monitor')) 
        else:
            result="Your logged in fail!!!"
                
    
    return render_template('login2.html',data1=data1,data2=data2)'''

#########################


        
@app.route('/register',methods=['POST','GET'])
def register():
    msg=""
    act=""
    email=""
    mess=""
    mycursor = mydb.cursor()
    
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()

    now = datetime.datetime.now()
    rdd =now.strftime("%d-%m-%Y")
    
    if request.method=='POST':
        name=request.form['name']
        dob=request.form['dob']
        gender=request.form['gender']
        mobile=request.form['mobile']
        email=request.form['email']
        address=request.form['address']
        city=request.form['city']
        pincode=request.form['pincode']
        
        vtype=request.form['vtype']
        vehicle=request.form['vehicle']                     
        vmodel=request.form['vmodel']
        vcolor=request.form['vcolor']

        vno=request.form['vno']
        regdate=request.form['regdate']                
        
        
        

        mycursor.execute("SELECT count(*) FROM ap_register where vno=%s",(vno, ))
        cnt = mycursor.fetchone()[0]
        if cnt==0:
            mycursor.execute("SELECT max(id)+1 FROM ap_register")
            maxid = mycursor.fetchone()[0]
            if maxid is None:
                maxid=1

            ff2=open("un1.txt","w")
            ff2.write(vno)
            ff2.close()

            file1 = request.files['file1']
            file2 = request.files['file2']
            file3 = request.files['file3']
            file4 = request.files['file4']
            
            fn1=file1.filename
            fnn1="A"+str(maxid)+fn1
            file1.save(os.path.join("static/upload", fnn1))

            fn2=file2.filename
            fnn2="D"+str(maxid)+fn2
            file2.save(os.path.join("static/upload", fnn2))

            fn3=file3.filename
            fnn3="P"+str(maxid)+fn3
            file3.save(os.path.join("static/upload", fnn3))

            fn4=file4.filename
            fnn4="V"+str(maxid)+fn4
            file4.save(os.path.join("static/upload", fnn4))

        

            na=name[0:3]
            uname=na+"00"+str(maxid)
            rn=randint(10000,99999)
            pass1=str(rn)

            mess="Dear "+name+" Your Vehicle No. "+vno+", Registerd in RTO - Username: "+uname+", Password: "+pass1
            
            sql = "INSERT INTO ap_register(id, vno, name, gender, dob, mobile, email, address, city, uname, pass,photo, rdate,vtype,vehicle,vmodel,vcolor,vehicle_photo,aadhar,driving,pincode) VALUES (%s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s, %s,%s,%s, %s, %s, %s,%s,%s)"
            val = (maxid, vno, name, gender, dob, mobile, email, address, city, uname, pass1,fnn3, regdate,vtype,vehicle,vmodel,vcolor,fnn4,fnn1,fnn2,pincode)
            print(sql)
            mycursor.execute(sql, val)
            mydb.commit()            
            print(mycursor.rowcount, "record inserted.")
            msg="success"
            
            
        else:
            result="Vehicle No. Already Exist!"
    return render_template('register.html',email=email,mess=mess,msg=msg,title=title,rdd=rdd)


@app.route('/userhome',methods=['POST','GET'])
def userhome():
    msg=""
    uname=""
    if 'username' in session:
        uname = session['username']
    mycursor = mydb.cursor()
  

    mycursor.execute("SELECT * FROM ap_register where uname=%s",(uname,))
    rs = mycursor.fetchone()

    return render_template('userhome.html',msg=msg,rs=rs)

@app.route('/user_update',methods=['POST','GET'])
def user_update():
    msg=""
    uname=""
    if 'username' in session:
        uname = session['username']
    mycursor = mydb.cursor()
  

    mycursor.execute("SELECT * FROM ap_register where uname=%s",(uname,))
    rs = mycursor.fetchone()
    if request.method=='POST':
        mobile2=request.form['mobile2']
        mobile3=request.form['mobile3']
        mycursor.execute("update ap_register set mobile2=%s,mobile3=%s where uname=%s",(mobile2,mobile3,uname))
        mydb.commit()
        msg="ok"
        

    return render_template('user_update.html',msg=msg,rs=rs)

@app.route('/user_history',methods=['POST','GET'])
def user_history():
    msg=""
    uname=""
    if 'username' in session:
        uname = session['username']
    mycursor = mydb.cursor()
  

    mycursor.execute("SELECT * FROM ap_register where uname=%s",(uname,))
    rs = mycursor.fetchone()
    vno=rs[1]

    mycursor.execute("SELECT * FROM ap_alert where vno=%s",(vno,))
    data = mycursor.fetchall()

    return render_template('user_history.html',msg=msg,rs=rs,data=data)


@app.route('/view_vehicle',methods=['POST','GET'])
def view_vehicle():
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    mycursor = mydb.cursor()
    
    mycursor.execute("SELECT * FROM ap_register")
    data = mycursor.fetchall()
    return render_template('view_vehicle.html', data=data)

@app.route('/add_location',methods=['POST','GET'])
def add_location():
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    mycursor = mydb.cursor()
    
    mycursor.execute("SELECT * FROM ap_register")
    data = mycursor.fetchall()
    return render_template('add_location.html', data=data)


@app.route('/view_member',methods=['POST','GET'])
def view_member():
    ff1=open("log.txt","r")
    uname=ff1.read()
    ff1.close()
    mycursor = mydb.cursor()
    mycursor.execute("SELECT id FROM ap_register where uname=%s",(uname,))
    rid = mycursor.fetchone()[0]

    mycursor.execute("SELECT * FROM ap_register where rid=%s",(rid,))
    result = mycursor.fetchall()
    return render_template('view_member.html', result=result)

###########
#VaDE
def VaDE():
    return np.asarray(X, dtype=theano.config.floatX)
    
def sampling(args):
    z_mean, z_log_var = args
    epsilon = K.random_normal(shape=(batch_size, latent_dim), mean=0.)
    return z_mean + K.exp(z_log_var / 2) * epsilon
#=====================================
def cluster_acc(Y_pred, Y):
  from sklearn.utils.linear_assignment_ import linear_assignment
  assert Y_pred.size == Y.size
  D = max(Y_pred.max(), Y.max())+1
  w = np.zeros((D,D), dtype=np.int64)
  for i in range(Y_pred.size):
    w[Y_pred[i], Y[i]] += 1
  ind = linear_assignment(w.max() - w)
  return sum([w[i,j] for i,j in ind])*1.0/Y_pred.size, w
             
#==================================================
def load_data(dataset):
    path = 'dataset/'+dataset+'/'
    if dataset == 'mnist':
        path = path + 'mnist.pkl.gz'
        if path.endswith(".gz"):
            f = gzip.open(path, 'rb')
        else:
            f = open(path, 'rb')
    
        if sys.version_info < (3,):
            (x_train, y_train), (x_test, y_test) = cPickle.load(f)
        else:
            (x_train, y_train), (x_test, y_test) = cPickle.load(f, encoding="bytes")
    
        f.close()
        x_train = x_train.astype('float32') / 255.
        x_test = x_test.astype('float32') / 255.
        x_train = x_train.reshape((len(x_train), np.prod(x_train.shape[1:])))
        x_test = x_test.reshape((len(x_test), np.prod(x_test.shape[1:])))
        X = np.concatenate((x_train,x_test))
        Y = np.concatenate((y_train,y_test))
        
    if dataset == 'reuters10k':
        data=scio.loadmat(path+'reuters10k.mat')
        X = data['X']
        Y = data['Y'].squeeze()
        
    if dataset == 'har':
        data=scio.loadmat(path+'HAR.mat')
        X=data['X']
        X=X.astype('float32')
        Y=data['Y']-1
        X=X[:10200]
        Y=Y[:10200]

    return X,Y

def config_init(dataset):
    if dataset == 'mnist':
        return 784,3000,10,0.002,0.002,10,0.9,0.9,1,'sigmoid'
    if dataset == 'reuters10k':
        return 2000,15,4,0.002,0.002,5,0.5,0.5,1,'linear'
    if dataset == 'har':
        return 561,120,6,0.002,0.00002,10,0.9,0.9,5,'linear'
        
def gmmpara_init():
    
    theta_init=np.ones(n_centroid)/n_centroid
    u_init=np.zeros((latent_dim,n_centroid))
    lambda_init=np.ones((latent_dim,n_centroid))
    
    theta_p=theano.shared(np.asarray(theta_init,dtype=theano.config.floatX),name="pi")
    u_p=theano.shared(np.asarray(u_init,dtype=theano.config.floatX),name="u")
    lambda_p=theano.shared(np.asarray(lambda_init,dtype=theano.config.floatX),name="lambda")
    return theta_p,u_p,lambda_p

#================================
def get_gamma(tempz):
    temp_Z=T.transpose(K.repeat(tempz,n_centroid),[0,2,1])
    temp_u_tensor3=T.repeat(u_p.dimshuffle('x',0,1),batch_size,axis=0)
    temp_lambda_tensor3=T.repeat(lambda_p.dimshuffle('x',0,1),batch_size,axis=0)
    temp_theta_tensor3=theta_p.dimshuffle('x','x',0)*T.ones((batch_size,latent_dim,n_centroid))
    
    temp_p_c_z=K.exp(K.sum((K.log(temp_theta_tensor3)-0.5*K.log(2*math.pi*temp_lambda_tensor3)-\
                       K.square(temp_Z-temp_u_tensor3)/(2*temp_lambda_tensor3)),axis=1))+1e-10
    return temp_p_c_z/K.sum(temp_p_c_z,axis=-1,keepdims=True)
#=====================================================
def vae_loss(x, x_decoded_mean):
    Z=T.transpose(K.repeat(z,n_centroid),[0,2,1])
    z_mean_t=T.transpose(K.repeat(z_mean,n_centroid),[0,2,1])
    z_log_var_t=T.transpose(K.repeat(z_log_var,n_centroid),[0,2,1])
    u_tensor3=T.repeat(u_p.dimshuffle('x',0,1),batch_size,axis=0)
    lambda_tensor3=T.repeat(lambda_p.dimshuffle('x',0,1),batch_size,axis=0)
    theta_tensor3=theta_p.dimshuffle('x','x',0)*T.ones((batch_size,latent_dim,n_centroid))
    
    p_c_z=K.exp(K.sum((K.log(theta_tensor3)-0.5*K.log(2*math.pi*lambda_tensor3)-\
                       K.square(Z-u_tensor3)/(2*lambda_tensor3)),axis=1))+1e-10

    gamma=p_c_z/K.sum(p_c_z,axis=-1,keepdims=True)
    gamma_t=K.repeat(gamma,latent_dim)
    
    if datatype == 'sigmoid':
        loss=alpha*original_dim * objectives.binary_crossentropy(x, x_decoded_mean)\
        +K.sum(0.5*gamma_t*(latent_dim*K.log(math.pi*2)+K.log(lambda_tensor3)+K.exp(z_log_var_t)/lambda_tensor3+K.square(z_mean_t-u_tensor3)/lambda_tensor3),axis=(1,2))\
        -0.5*K.sum(z_log_var+1,axis=-1)\
        -K.sum(K.log(K.repeat_elements(theta_p.dimshuffle('x',0),batch_size,0))*gamma,axis=-1)\
        +K.sum(K.log(gamma)*gamma,axis=-1)
    else:
        loss=alpha*original_dim * objectives.mean_squared_error(x, x_decoded_mean)\
        +K.sum(0.5*gamma_t*(latent_dim*K.log(math.pi*2)+K.log(lambda_tensor3)+K.exp(z_log_var_t)/lambda_tensor3+K.square(z_mean_t-u_tensor3)/lambda_tensor3),axis=(1,2))\
        -0.5*K.sum(z_log_var+1,axis=-1)\
        -K.sum(K.log(K.repeat_elements(theta_p.dimshuffle('x',0),batch_size,0))*gamma,axis=-1)\
        +K.sum(K.log(gamma)*gamma,axis=-1)
        
    return loss
#================================

def load_pretrain_weights(vade,dataset):
    ae = model_from_json(open('pretrain_weights/ae_'+dataset+'.json').read())
    ae.load_weights('pretrain_weights/ae_'+dataset+'_weights.h5')
    vade.layers[1].set_weights(ae.layers[0].get_weights())
    vade.layers[2].set_weights(ae.layers[1].get_weights())
    vade.layers[3].set_weights(ae.layers[2].get_weights())
    vade.layers[4].set_weights(ae.layers[3].get_weights())
    vade.layers[-1].set_weights(ae.layers[-1].get_weights())
    vade.layers[-2].set_weights(ae.layers[-2].get_weights())
    vade.layers[-3].set_weights(ae.layers[-3].get_weights())
    vade.layers[-4].set_weights(ae.layers[-4].get_weights())
    sample = sample_output.predict(X,batch_size=batch_size)
    if dataset == 'mnist':
        g = mixture.GMM(n_components=n_centroid,covariance_type='diag')
        g.fit(sample)
        u_p.set_value(floatX(g.means_.T))
        lambda_p.set_value((floatX(g.covars_.T)))
    if dataset == 'reuters10k':
        k = KMeans(n_clusters=n_centroid)
        k.fit(sample)
        u_p.set_value(floatX(k.cluster_centers_.T))
    if dataset == 'har':
        g = mixture.GMM(n_components=n_centroid,covariance_type='diag',random_state=3)
        g.fit(sample)
        u_p.set_value(floatX(g.means_.T))
        lambda_p.set_value((floatX(g.covars_.T)))
    print ('pretrain weights loaded!')
    return vade
#===================================
def lr_decay():
    if dataset == 'mnist':
        adam_nn.lr.set_value(floatX(max(adam_nn.lr.get_value()*decay_nn,0.0002)))
        adam_gmm.lr.set_value(floatX(max(adam_gmm.lr.get_value()*decay_gmm,0.0002)))
    else:
        adam_nn.lr.set_value(floatX(adam_nn.lr.get_value()*decay_nn))
        adam_gmm.lr.set_value(floatX(adam_gmm.lr.get_value()*decay_gmm))
    print ('lr_nn:%f'%adam_nn.lr.get_value())
    print ('lr_gmm:%f'%adam_gmm.lr.get_value())
    
def epochBegin(epoch):

    if epoch % decay_n == 0 and epoch!=0:
        lr_decay()
    '''
    sample = sample_output.predict(X,batch_size=batch_size)
    g = mixture.GMM(n_components=n_centroid,covariance_type='diag')
    g.fit(sample)
    p=g.predict(sample)
    acc_g=cluster_acc(p,Y)
    
    if epoch <1 and ispretrain == False:
        u_p.set_value(floatX(g.means_.T))
        print ('no pretrain,random init!')
    '''
    gamma = gamma_output.predict(X,batch_size=batch_size)
    acc=cluster_acc(np.argmax(gamma,axis=1),Y)
    global accuracy
    accuracy+=[acc[0]]
    if epoch>0 :
        #print ('acc_gmm_on_z:%0.8f'%acc_g[0])
        print ('acc_p_c_z:%0.8f'%acc[0])
    if epoch==1 and dataset == 'har' and acc[0]<0.77:
        print ('=========== HAR dataset:bad init!Please run again! ============')
        sys.exit(0)
        

def on_epoch_begin():
       
    dataset = 'mnist'
    db = sys.argv[1]
    if db in ['mnist','reuters10k','har']:
        dataset = db
    print ('training on: ' + dataset)
    ispretrain = True
    batch_size = 100
    latent_dim = 10
    intermediate_dim = [500,500,2000]
    theano.config.floatX='float32'
    accuracy=[]
    X,Y = load_data(dataset)
    original_dim,epoch,n_centroid,lr_nn,lr_gmm,decay_n,decay_nn,decay_gmm,alpha,datatype = config_init(dataset)
    theta_p,u_p,lambda_p = gmmpara_init()
    #===================

    x = Input(batch_shape=(batch_size, original_dim))
    h = Dense(intermediate_dim[0], activation='relu')(x)
    h = Dense(intermediate_dim[1], activation='relu')(h)
    h = Dense(intermediate_dim[2], activation='relu')(h)
    z_mean = Dense(latent_dim)(h)
    z_log_var = Dense(latent_dim)(h)
    z = Lambda(sampling, output_shape=(latent_dim,))([z_mean, z_log_var])
    h_decoded = Dense(intermediate_dim[-1], activation='relu')(z)
    h_decoded = Dense(intermediate_dim[-2], activation='relu')(h_decoded)
    h_decoded = Dense(intermediate_dim[-3], activation='relu')(h_decoded)
    x_decoded_mean = Dense(original_dim, activation=datatype)(h_decoded)

    #========================
    Gamma = Lambda(get_gamma, output_shape=(n_centroid,))(z)
    sample_output = Model(x, z_mean)
    gamma_output = Model(x,Gamma)
    #===========================================      
    vade = Model(x, x_decoded_mean)
    if ispretrain == True:
        vade = load_pretrain_weights(vade,dataset)
    adam_nn= Adam(lr=lr_nn,epsilon=1e-4)
    adam_gmm= Adam(lr=lr_gmm,epsilon=1e-4)
    vade.compile(optimizer=adam_nn, loss=vae_loss,add_trainable_weights=[theta_p,u_p,lambda_p],add_optimizer=adam_gmm)
    epoch_begin=EpochBegin()
    #-------------------------------------------------------

    vade.fit(X, X,
            shuffle=True,
            nb_epoch=epoch,
            batch_size=batch_size,   
            callbacks=[epoch_begin])

def get_gamma(tempz):
    temp_Z=T.transpose(K.repeat(tempz,n_centroid),[0,2,1])
    temp_u_tensor3=T.repeat(u_p.dimshuffle('x',0,1),batch_size,axis=0)
    temp_lambda_tensor3=T.repeat(lambda_p.dimshuffle('x',0,1),batch_size,axis=0)
    temp_theta_tensor3=theta_p.dimshuffle('x','x',0)*T.ones((batch_size,latent_dim,n_centroid))
    
    temp_p_c_z=K.exp(K.sum((K.log(temp_theta_tensor3)-0.5*K.log(2*math.pi*temp_lambda_tensor3)-\
                       K.square(temp_Z-temp_u_tensor3)/(2*temp_lambda_tensor3)),axis=1))
    return temp_p_c_z/K.sum(temp_p_c_z,axis=-1,keepdims=True)
    #=====================================================

    ispretrain = True
    batch_size = 100
    latent_dim = 10
    intermediate_dim = [500,500,2000]
    theano.config.floatX='float32'
    X,Y = load_data()
    original_dim = 2000
    n_centroid = 4 
    theta_p, u_p, lambda_p = gmm_para_init()
    #===================

    x = Input(batch_shape=(batch_size, original_dim))
    h = Dense(intermediate_dim[0])(x)
    h=Activation('relu')(h)
    h=Dropout(0.2)(h)
    h = Dense(intermediate_dim[1])(h)
    h=Activation('relu')(h)
    h=Dropout(0.2)(h)
    h = Dense(intermediate_dim[2])(h)
    h=Activation('relu')(h)
    h=Dropout(0.2)(h)
    z_mean = Dense(latent_dim)(h)
    z_log_var = Dense(latent_dim)(h)
    z = Lambda(sampling, output_shape=(latent_dim,))([z_mean, z_log_var])
    z = Lambda(sampling, output_shape=(latent_dim,))([z_mean, z_log_var])
    h_decoded = Dense(intermediate_dim[-1])(z)
    h_decoded = Dense(intermediate_dim[-2])(h_decoded)
    h_decoded = Dense(intermediate_dim[-3])(h_decoded)
    x_decoded_mean = Dense(original_dim)(h_decoded)

    #========================
    p_c_z = Lambda(get_gamma, output_shape=(n_centroid,))(z_mean)
    sample_output = Model(x, z_mean)
    p_c_z_output = Model(x, p_c_z)
    #===========================================      
    vade = Model(x, x_decoded_mean)
    vade.load_weights('trained_model_weights/reuters_all_weights_nn.h5')

    accuracy = cluster_acc(np.argmax(p_c_z_output.predict(X,batch_size=batch_size),axis=1),Y)
#############


@app.route('/get_loc', methods=['GET', 'POST'])
def get_loc():
    msg=""
    aid=""
    fid=request.args.get("fid")
    ctype=request.args.get("ctype")
    mycursor = mydb.cursor()
    if request.method=='POST':
        detail=request.form['detail']
        location=request.form['location']
        city=request.form['city']
        nh_number=request.form['nh_number']

        #n1=len(detail)
        #n2=n1-3
        #value=detail[1:n2]
        #print(value)
        
        mycursor.execute("SELECT max(id)+1 FROM ap_geo_location")
        maxid = mycursor.fetchone()[0]
        if maxid is None:
            maxid=1

        
        sql = "INSERT INTO ap_geo_location(id,location,detail,city,nh_number) VALUES (%s, %s, %s,%s,%s)"
        val = (maxid,location,detail,city,nh_number)
        act="success"
        mycursor.execute(sql, val)
        mydb.commit()
        aid=str(maxid)
        ##
        d1=detail.split('new google.maps.LatLng(')
        dlen=len(d1)

        
        i=1
        while i<dlen:
            d2=d1[i]
            d3=d2.split('), ')
            
            d4=d3[0].split(',')

            mycursor.execute("SELECT max(id)+1 FROM ap_location_data")
            maxid2 = mycursor.fetchone()[0]
            if maxid2 is None:
                maxid2=1

            
            sql = "INSERT INTO ap_location_data(id,gid,area,city,latitude,longitude,nh_number) VALUES (%s, %s, %s,%s,%s,%s,%s)"
            val = (maxid2,maxid,location,city,d4[0],d4[1],nh_number)
            act="success"
            mycursor.execute(sql, val)
            mydb.commit()
            
            i+=1
        ##
        msg="ok"

    mycursor.execute('SELECT * FROM ap_geo_location order by id desc limit 0,1')
    view=mycursor.fetchone()

    mycursor.execute('SELECT * FROM ap_geo_location where test_st=0')
    data2=mycursor.fetchall()

    '''for dat in data2:
        nh=dat[4]
        mycursor.execute("update ap_location_data set nh_number=%s where gid=%s",(nh,dat[0]))
        mydb.commit()'''

    
    
    return render_template('get_loc.html',fid=fid,ctype=ctype,msg=msg,view=view,aid=aid,data2=data2)

@app.route('/map1', methods=['GET', 'POST'])
def map1():
    msg=""
    aid=request.args.get("aid")
    lat=""
    lon=""
    mycursor = mydb.cursor()

    mycursor.execute('SELECT * FROM ap_geo_location where id=%s',(aid,))
    data1=mycursor.fetchone()

    
    mycursor.execute('SELECT * FROM ap_location_data where gid=%s',(aid,))
    data=mycursor.fetchall()

    lat2=data[0][4]
    lon2=data[0][5]
   

    x=0
    y=0
    i=0
    for dd in data:
        la=dd[4].split(".")
        lb=dd[5].split(".")

        laa=la[0]
        lbb=lb[0]

        vx1=la[1]
        vy1=lb[1]
        if len(vx1)>=6 and len(vy1)>=6:
            vx2=vx1[0:6]
            vy2=vy1[0:6]

            x+=int(vx2)
            y+=int(vy2)

            i+=1


    dx1=x/i
    dx2=math.ceil(dx1)

    dy1=y/i
    dy2=math.ceil(dy1)

    v1=dx2-400
    v2=dy2-300

    lat=laa+"."+str(v1)
    lon=lbb+"."+str(v2)

    #lat="10.826108"
    #lon="78.712139"
        
    #print(lat)
    #print(lon)

    #ds=['13','2','','',lat,lon]
    #data.append(ds)
    if request.method=='POST':

        
        num_acc=request.form.getlist("num_acc[]")
        rid=request.form.getlist("rid[]")
        i=0
        for ridd in rid:
            mycursor.execute("update ap_location_data set num_accident=%s where id=%s",(num_acc[i],ridd))
            mydb.commit()
            i+=1

        ###
        mycursor.execute("update ap_location_data set area=%s,city=%s,nh_number=%s where id=%s",(data1[1],data1[3],data1[4],aid))
        mydb.commit()
        
        mycursor.execute("update ap_geo_location set lat=%s,lon=%s where id=%s",(lat,lon,aid))
        mydb.commit()
        ###
        mycursor.execute("SELECT id,gid,area,city,latitude,longitude,num_accident,nh_number FROM ap_location_data where test_st=0")
        result = mycursor.fetchall()
        with open('accident_data.csv','w') as outfile:
            writer = csv.writer(outfile, quoting=csv.QUOTE_NONNUMERIC)
            writer.writerow(col[0] for col in mycursor.description)
            for row in result:
                writer.writerow(row)

        with open('accident_data.csv') as input, open('accident_data.csv', 'w', newline='') as outfile:
            writer = csv.writer(outfile, quoting=csv.QUOTE_NONNUMERIC)
            writer.writerow(col[0] for col in mycursor.description)
            for row in result:
                if row or any(row) or any(field.strip() for field in row):
                    writer.writerow(row)
        ###
        msg="ok"
        
    
    return render_template('map1.html',msg=msg,data=data,lat=lat,lon=lon,lat2=lat2,lon2=lon2,aid=aid)

@app.route('/map2', methods=['GET', 'POST'])
def map2():
    msg=""
    aid=request.args.get("aid")
    lat2=""
    lon2=""
    mycursor = mydb.cursor()
    mycursor.execute("SELECT * FROM ap_location_data where test_st=0")
    data=mycursor.fetchall()
    lat2=data[0][4]
    lon2=data[0][5]
    
    mycursor.execute("SELECT * FROM ap_geo_location where test_st=0")
    data2=mycursor.fetchall()

  
    
    return render_template('map2.html',msg=msg,data=data,data2=data2,lat2=lat2,lon2=lon2)

@app.route('/view_location', methods=['GET', 'POST'])
def view_location():
    data=""
    msg=""
    aid=request.args.get("aid")
    
    return render_template('view_location.html',aid=aid)


@app.route('/map3', methods=['GET', 'POST'])
def map3():
    msg=""
    aid=request.args.get("aid")
    lat=""
    lon=""
    lat3=""
    lon3=""
    mycursor = mydb.cursor()
    mycursor.execute('SELECT * FROM ap_location_data where gid=%s',(aid,))
    data=mycursor.fetchall()

    lat2=data[0][4]
    lon2=data[0][5]
    
    rn1=0
    n=0
    for dw in data:
        n+=1

    rn=randint(1,n)
    rn1=rn

    x=0
    y=0
    i=0
    for dd in data:
        la=dd[4].split(".")
        lb=dd[5].split(".")

        laa=la[0]
        lbb=lb[0]

        vx1=la[1]
        vy1=lb[1]
        if len(vx1)>=6 and len(vy1)>=6:
            vx2=vx1[0:6]
            vy2=vy1[0:6]

            x+=int(vx2)
            y+=int(vy2)

            if rn1==i:
                la2=int(la[1])+30
                lb2=int(lb[1])+30
                lat3=laa+"."+str(la2)
                lon3=lbb+"."+str(lb2)
                

            i+=1

    
    dx1=x/i
    dx2=math.ceil(dx1)



    dy1=y/i
    dy2=math.ceil(dy1)

    v1=dx2-400
    v2=dy2-300

    lat=laa+"."+str(v1)
    lon=lbb+"."+str(v2)

    #lat="10.826108"
    #lon="78.712139"
        
    print(lat)
    print(lon)

    ####
    ff=open("static/amno.txt","r")
    det=ff.read()
    ff.close()
    dett=det.split(",")
    value="Ambulance No.: "+dett[0]+", Driver: "+dett[1]
    #####

    #ds=['13','2','','',lat,lon]
    #data.append(ds)
    
    return render_template('map3.html',msg=msg,data=data,lat=lat,lon=lon,lat2=lat2,lon2=lon2,aid=aid,lat3=lat3,lon3=lon3,value=value)




    
@app.route('/vehicle_move',methods=['POST','GET'])
def vehicle_move():
    msg=""
    aid=""
    amno=""
    driver=""
    area=""
    city=""
    ff11=open("title.txt","r")
    title=ff11.read()
    ff11.close()
    
    ff2=open("un.txt","r")
    vno=ff2.read()
    ff2.close()

    speed=20
    speed2=20
    rnv=0
    vnn=[]
    data1=""
    mess=""
    mobile=""
    mobile2=""
    mobile3=""
    mobile4=""
    name2=""
    act = request.args.get('act')
    st = request.args.get('st')
    tt = request.args.get('tt')
    cursor = mydb.cursor()
    
    cursor.execute('SELECT * FROM ap_register WHERE vno = %s', (vno, ))
    data = cursor.fetchone()
    name=data[2]
    vno=data[1]
    mobile=data[5]
    city=data[10]

    
    ####
    vnn=[]
    cursor.execute('SELECT * FROM ap_register WHERE vno != %s', (vno, ))
    data2 = cursor.fetchall()
    for ds2 in data2:
        ut='admin'
        cursor.execute('SELECT count(*) FROM ap_register WHERE vno=%s',(ds2[1],))
        dn3 = cursor.fetchone()[0]
        if dn3>0:
            cursor.execute('SELECT * FROM ap_register WHERE vno=%s',(ds2[1],))
            dd3 = cursor.fetchone()
            vnn.append(dd3[1])
        
    lv=len(vnn)
    if lv>0:
        cursor.execute('SELECT * FROM ap_register WHERE vno = %s', (vnn[0], ))
        data3 = cursor.fetchone()
        #mobile2=data3[5]
        name2=data3[2]

    
    
        
   
    val=[30,40,50,60,70,80,100,110,120]
    rn=randint(0,8)
    speed=val[rn]

    ##
    aid=""
    cursor.execute("SELECT * FROM ap_geo_location where id>1 && test_st=0 order by rand()")
    grow = cursor.fetchall()

    for grow2 in grow:
        aid=str(grow2[0])

    ##
    if aid=="":
        s=1
    else:
        cursor.execute("SELECT * FROM ap_geo_location where id=%s",(aid,))
        grow1 = cursor.fetchall()
        for grow11 in grow1:
            area=grow11[1]
            city=grow11[3]


    
    st=""
    if speed>=70:
        rnv=randint(1,20)
        rnv2=12
        print(rnv)
        if rnv>rnv2:
            st="1"
            tt="yes"
            msg="Accident Occured, Vehicle No.:"+vno
            data1=msg
            mess="Accident Occured, Vehicle No.:"+vno

            ##
    
            cursor.execute('SELECT * FROM ap_ambulance where area=%s and city=%s',(area,city))
            mdat1 = cursor.fetchall()
            for mdat11 in mdat1:
                mobile2=mdat11[5]
                driver=mdat11[4]
                amno=mdat11[1]

            det=amno+","+driver
            ff=open("static/amno.txt","w")
            ff.write(det)
            ff.close()

            cursor.execute('SELECT * FROM ap_hospital where city=%s',(city,))
            mdat2 = cursor.fetchall()
            for mdat22 in mdat2:
                mobile3=mdat22[4]

            cursor.execute('SELECT * FROM ap_traffic where location=%s and city=%s',(area,city))
            mdat3 = cursor.fetchall()
            for mdat33 in mdat3:
                mobile4=mdat33[6]
            ##
    
            #url="http://iotcloud.co.in/testsms/sms.php?sms=emr&name="+name+"&mess="+mess+"&mobile="+str(mobile)
            #webbrowser.open_new(url)
            if mobile2=="":
                s=1
            else:
                s=1
                #url="http://iotcloud.co.in/testsms/sms.php?sms=emr&name="+name2+"&mess="+mess+"&mobile="+str(mobile2)
                #webbrowser.open_new(url)

        else:
            st="0"

        cursor.execute("SELECT max(id)+1 FROM ap_alert")
        maxid = cursor.fetchone()[0]
        if maxid is None:
            maxid=1
        sql = "INSERT INTO ap_alert(id, vno, vspeed,acc_st,city) VALUES (%s, %s, %s,%s,%s)"
        val = (maxid, vno, speed,st,city)
        print(sql)
        cursor.execute(sql, val)
        mydb.commit()      
    

        
    
    return render_template('vehicle_move.html', msg=msg,data=data,data1=data1,act=act,speed=speed,speed2=speed2,rnv=rnv,st=st,tt=tt,vnn=vnn,title=title,mess=mess,mobile=mobile,mobile2=mobile2,mobile3=mobile3,mobile4=mobile4,aid=aid)

@app.route('/logout')
def logout():
    # remove the username from the session if it is there
    #session.pop('username', None)
    return redirect(url_for('index'))


if __name__ == "__main__":
    app.secret_key = os.urandom(12)
    app.run(debug=True,host='0.0.0.0', port=5000)
