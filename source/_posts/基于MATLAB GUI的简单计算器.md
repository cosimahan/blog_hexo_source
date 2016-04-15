layout: 
title: 基于MATLAB GUI的简单计算器
date: 2015-05-17 15:09:25
tags:
---
首先网上搜索，大概了解MATLAB GUI，然后开始做。

我的目标是仿照普通实体计算器的样子和功能实现一个简单的计算器。

进入GUI Developer之后，先添加几个text框，分别是作为输出的“o1”和寄存数据用的“t1”，为了美观，又使用一个“我是计算器”文本框把t1挡住。

接下来添加按键，分别是数字键0-9、四则运算键加减乘除、储存功能键M+ M- MC MR，再加上AC退格和等于键。
![](http://7u2p8s.com1.z0.glb.clouddn.com/blog/img/2016/01/1.png)

然后该写按键触发所执行的代码了，大致思路如下

1.	输入输出部分：按下数字键，先判断如果是开始新的计算，则把o1清零。把o1的值乘以10,再加上所按下的数字。
以“1”为例，代码如下
![](http://7u2p8s.com1.z0.glb.clouddn.com/blog/img/2016/01/2.png)

2.	四则运算部分：按下运算键，先判断有无待执行的运算，若有则执行，然后把自己设为待执行的运算，如果按下的是等号，就直接执行前面待执行的运算好了，以“+”为例，代码如下
![](http://7u2p8s.com1.z0.glb.clouddn.com/blog/img/2016/01/3.png)
![](http://7u2p8s.com1.z0.glb.clouddn.com/blog/img/2016/01/4.png)

3.	寄存部分：就是搞一个全局变量，然后对它进行操作，代码如下：
![](http://7u2p8s.com1.z0.glb.clouddn.com/blog/img/2016/01/5.png)

4.	还剩下两个键，AC就把除了寄存相关的都初始化，退格就是把o1里的量与它自身对10的模的差除以10。

5.	本来想加小数输入功能来着，感觉不难，懒得写了。

附完整代码：
```
function varargout = c2(varargin)
% C2 MATLAB code for c2.fig
%      C2, by itself, creates a new C2 or raises the existing
%      singleton*.
%
%      H = C2 returns the handle to a new C2 or the handle to
%      the existing singleton*.
%
%      C2('CALLBACK',hObject,eventData,handles,...) calls the local
%      function named CALLBACK in C2.M with the given input arguments.
%
%      C2('Property','Value',...) creates a new C2 or raises the
%      existing singleton*.  Starting from the left, property value pairs are
%      applied to the GUI before c2_OpeningFcn gets called.  An
%      unrecognized property name or invalid value makes property application
%      stop.  All inputs are passed to c2_OpeningFcn via varargin.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help c2

% Last Modified by GUIDE v2.5 28-Apr-2015 22:50:09

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @c2_OpeningFcn, ...
                   'gui_OutputFcn',  @c2_OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT
global k;




% --- Executes just before c2 is made visible.
function c2_OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   command line arguments to c2 (see VARARGIN)

% Choose default command line output for c2
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes c2 wait for user response (see UIRESUME)
% uiwait(handles.figure1);
global k;
global k1;
global k2;
global k3;
global k4;
k=0;
k1=0;
k2=0;
k3=0;
k4=0;





% --- Outputs from this function are returned to the command line.
function varargout = c2_OutputFcn(hObject, eventdata, handles) 
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;


% --- Executes on button press in pushbutton01.
function pushbutton01_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton01 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
k=1;
%a=get(handles.t1,'String');
%b=get(handles.t2,'String'); 
%anss=str2num(a)+str2num(b);
%c=num2str(anss); 
%set(handles.t3,'String',c);
%guidata(hObject,handles);



function t1_Callback(hObject, eventdata, handles)
% hObject    handle to t1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of t1 as text
%        str2double(get(hObject,'String')) returns contents of t1 as a double
input=str2num(get(hObject,'String'));
if (isempty(input))   
    set(hObject,'String','0') 
end
guidata(hObject,handles)


% --- Executes during object creation, after setting all properties.
function t1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to t1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end



function t2_Callback(hObject, eventdata, handles)
% hObject    handle to t2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of t2 as text
%        str2double(get(hObject,'String')) returns contents of t2 as a double
input=str2num(get(hObject,'String'));
if (isempty(input))   
    set(hObject,'String','0') 
end
guidata(hObject,handles)



% --- Executes during object creation, after setting all properties.
function t2_CreateFcn(hObject, eventdata, handles)
% hObject    handle to t2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end



function t3_Callback(hObject, eventdata, handles)
% hObject    handle to t3 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of t3 as text
%        str2double(get(hObject,'String')) returns contents of t3 as a double
input=str2num(get(hObject,'String'));
if (isempty(input))   
    set(hObject,'String','0') 
end
guidata(hObject,handles)



% --- Executes during object creation, after setting all properties.
function t3_CreateFcn(hObject, eventdata, handles)
% hObject    handle to t3 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on button press in pushbutton02.
function pushbutton02_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton02 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

global k;
k=2;
%a=get(handles.t1,'String');
%b=get(handles.t2,'String'); 
%anss=str2num(a)-str2num(b);
%c=num2str(anss); 
%set(handles.t3,'String',c);
%guidata(hObject,handles);

% --- Executes on button press in pushbutton03.
function pushbutton03_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton03 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
k=3;
%a=get(handles.t1,'String');
%b=get(handles.t2,'String'); 
%anss=str2num(a)*str2num(b);
%c=num2str(anss); 
%set(handles.t3,'String',c);
%guidata(hObject,handles);

% --- Executes on button press in pushbutton04.
function pushbutton04_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton04 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
k=4;
%a=get(handles.t1,'String');
%b=get(handles.t2,'String'); 
%anss=str2num(a)/str2num(b);
%c=num2str(anss); 
%set(handles.t3,'String',c);
%guidata(hObject,handles);


% --- Executes on button press in pushbutton1.
function pushbutton1_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+1;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+1;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton2.
function pushbutton2_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+2;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+2;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton3.
function pushbutton3_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton3 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+3;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+3;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton4.
function pushbutton4_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton4 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+4;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+4;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton5.
function pushbutton5_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton5 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+5;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+5;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton6.
function pushbutton6_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton6 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+6;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+6;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton7.
function pushbutton7_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton7 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+7;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+7;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton8.
function pushbutton8_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton8 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+8;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+8;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton9.
function pushbutton9_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton9 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+9;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+9;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton0.
function pushbutton0_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton0 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*10+0;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*10+0;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton00.
function pushbutton00_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton00 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
if(k)
    
    b=get(handles.t2,'String');
    o=str2num(b);
    o=o*100+0;
    b=num2str(o); 
    set(handles.t2,'String',b);
    set(handles.o1,'String',b);
else
    
    a=get(handles.t1,'String');
    o=str2num(a);
    o=o*100+0;
    a=num2str(o); 
    set(handles.t1,'String',a);
    set(handles.o1,'String',a);
end


% --- Executes on button press in pushbutton0p.
function pushbutton0p_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton0p (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)




function edit4_Callback(hObject, eventdata, handles)
% hObject    handle to edit4 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of edit4 as text
%        str2double(get(hObject,'String')) returns contents of edit4 as a double


% --- Executes during object creation, after setting all properties.
function edit4_CreateFcn(hObject, eventdata, handles)
% hObject    handle to edit4 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end



function edit5_Callback(hObject, eventdata, handles)
% hObject    handle to edit5 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of edit5 as text
%        str2double(get(hObject,'String')) returns contents of edit5 as a double


% --- Executes during object creation, after setting all properties.
function edit5_CreateFcn(hObject, eventdata, handles)
% hObject    handle to edit5 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on button press in pushbutton0e.
function pushbutton0e_Callback(hObject, ~, handles)
% hObject    handle to pushbutton0e (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
global k;
global k1;
global k2;
global k3;
global k4;
if(k==1)
    a=get(handles.t1,'String');
    b=get(handles.t2,'String'); 
    anss=str2num(a)+str2num(b);
    c=num2str(anss); 
    set(handles.o1,'String',c);
  
else
    if(k==2)
        a=get(handles.t1,'String');
        b=get(handles.t2,'String'); 
        anss=str2num(a)-str2num(b);
        c=num2str(anss); 
        set(handles.o1,'String',c);
        guidata(hObject,handles);

    else
        if(k==3)
            a=get(handles.t1,'String');
            b=get(handles.t2,'String'); 
            anss=str2num(a)*str2num(b);
            c=num2str(anss); 
            set(handles.o1,'String',c);
            guidata(hObject,handles);
        else
            if(k==4)
                a=get(handles.t1,'String');
                b=get(handles.t2,'String'); 
                anss=str2num(a)/str2num(b);
                c=num2str(anss); 
                set(handles.o1,'String',c);
                guidata(hObject,handles);
            end
        end
    end
end
k=0;
set(handles.t2,'String','0');
set(handles.t1,'String','0');



% --- Executes during object creation, after setting all properties.
function figure1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to figure1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called


% --- Executes during object creation, after setting all properties.
function o1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to o1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called


% --- Executes on button press in pushbutton0ac.
function pushbutton0ac_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton0ac (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
anss=0;
global k;
global k1;
global k2;
global k3;
global k4;
k=0;
k1=0;
k2=0;
k3=0;
k4=0;
oo=num2str(anss); 
set(handles.o1,'String','Hello, World');
set(handles.t1,'String','0');
set(handles.t2,'String','0');


% --- Executes on button press in pushbutton0t.
function pushbutton0t_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton0t (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
    a=get(handles.t1,'String');
    b=get(handles.t2,'String'); 
    anss=str2num(a)+str2num(b);
    c=num2str(anss); 
    set(handles.o1,'String',c);


% --- Executes on button press in pushbutton21.
function pushbutton21_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton21 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
helpdlg('helpstring','dlgname') ;


% --- Executes on button press in name.
function name_Callback(hObject, eventdata, handles)
% hObject    handle to name (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)



% --- Executes during object deletion, before destroying properties.
function name_DeleteFcn(hObject, eventdata, handles)
% hObject    handle to name (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
helpdlg('TEST MSG.') 

```