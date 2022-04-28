#Проект товароучëтной программы для ккт атолл. Используется библиотека libfpt10 и tkinter
#реализована возможность снятие диагностических данны, отчëтов, разрабатывается печать чеков
# 54FZ-project
from libfptr10 import IFptr
from tkinter import *

# Можно загрузить обертку с указанием пути к библиотеке драйвера
fptr = IFptr(r"C:\Program Files (x86)\ATOL\Drivers10\KKT\bin\fptr10.dll")


# Настройка драйвера
def a():
    settings = {
        IFptr.LIBFPTR_SETTING_MODEL: IFptr.LIBFPTR_MODEL_ATOL_AUTO,
        IFptr.LIBFPTR_SETTING_PORT: IFptr.LIBFPTR_PORT_COM,
        IFptr.LIBFPTR_SETTING_COM_FILE: com,
        IFptr.LIBFPTR_SETTING_BAUDRATE: IFptr.LIBFPTR_PORT_BR_115200
    }
    fptr.setSettings(settings)
    print(a)


# Установка соединения с ККТ
fptr.open()


# проверка связи с ккт
def b():
    isOpened = fptr.isOpened()
    print(isOpened)


# общая информация о ккт
def c():
    fptr.setParam(IFptr.LIBFPTR_PARAM_DATA_TYPE, IFptr.LIBFPTR_DT_STATUS)
    fptr.queryData()

    operatorID = fptr.getParamInt(IFptr.LIBFPTR_PARAM_OPERATOR_ID)
    logicalNumber = fptr.getParamInt(IFptr.LIBFPTR_PARAM_LOGICAL_NUMBER)
    shiftState = fptr.getParamInt(IFptr.LIBFPTR_PARAM_SHIFT_STATE)
    model = fptr.getParamInt(IFptr.LIBFPTR_PARAM_MODEL)
    mode = fptr.getParamInt(IFptr.LIBFPTR_PARAM_MODE)
    submode = fptr.getParamInt(IFptr.LIBFPTR_PARAM_SUBMODE)
    receiptNumber = fptr.getParamInt(IFptr.LIBFPTR_PARAM_RECEIPT_NUMBER)
    documentNumber = fptr.getParamInt(IFptr.LIBFPTR_PARAM_DOCUMENT_NUMBER)
    shiftNumber = fptr.getParamInt(IFptr.LIBFPTR_PARAM_SHIFT_NUMBER)
    receiptType = fptr.getParamInt(IFptr.LIBFPTR_PARAM_RECEIPT_TYPE)
    documentType = fptr.getParamInt(IFptr.LIBFPTR_PARAM_DOCUMENT_TYPE)
    lineLength = fptr.getParamInt(IFptr.LIBFPTR_PARAM_RECEIPT_LINE_LENGTH)
    lineLengthPix = fptr.getParamInt(IFptr.LIBFPTR_PARAM_RECEIPT_LINE_LENGTH_PIX)

    receiptSum = fptr.getParamDouble(IFptr.LIBFPTR_PARAM_RECEIPT_SUM)

    isFiscalDevice = fptr.getParamBool(IFptr.LIBFPTR_PARAM_FISCAL)
    isFiscalFN = fptr.getParamBool(IFptr.LIBFPTR_PARAM_FN_FISCAL)
    isFNPresent = fptr.getParamBool(IFptr.LIBFPTR_PARAM_FN_PRESENT)
    isInvalidFN = fptr.getParamBool(IFptr.LIBFPTR_PARAM_INVALID_FN)
    isCashDrawerOpened = fptr.getParamBool(IFptr.LIBFPTR_PARAM_CASHDRAWER_OPENED)
    isPaperPresent = fptr.getParamBool(IFptr.LIBFPTR_PARAM_RECEIPT_PAPER_PRESENT)
    isPaperNearEnd = fptr.getParamBool(IFptr.LIBFPTR_PARAM_PAPER_NEAR_END)
    isCoverOpened = fptr.getParamBool(IFptr.LIBFPTR_PARAM_COVER_OPENED)
    isPrinterConnectionLost = fptr.getParamBool(IFptr.LIBFPTR_PARAM_PRINTER_CONNECTION_LOST)
    isPrinterError = fptr.getParamBool(IFptr.LIBFPTR_PARAM_PRINTER_ERROR)
    isCutError = fptr.getParamBool(IFptr.LIBFPTR_PARAM_CUT_ERROR)
    isPrinterOverheat = fptr.getParamBool(IFptr.LIBFPTR_PARAM_PRINTER_OVERHEAT)
    isDeviceBlocked = fptr.getParamBool(IFptr.LIBFPTR_PARAM_BLOCKED)

    dateTime = fptr.getParamDateTime(IFptr.LIBFPTR_PARAM_DATE_TIME)

    serialNumber = fptr.getParamString(IFptr.LIBFPTR_PARAM_SERIAL_NUMBER)
    modelName = fptr.getParamString(IFptr.LIBFPTR_PARAM_MODEL_NAME)
    firmwareVersion = fptr.getParamString(IFptr.LIBFPTR_PARAM_UNIT_VERSION)
    print(shiftState)


# X-отчет
def d():
    fptr.setParam(IFptr.LIBFPTR_PARAM_REPORT_TYPE, IFptr.LIBFPTR_RT_X)
    fptr.report()


# Печать информации о ККТ
def e():
    fptr.setParam(IFptr.LIBFPTR_PARAM_REPORT_TYPE, IFptr.LIBFPTR_RT_KKT_INFO)
    fptr.report()


# регистрация чека
    
def enter():
    a = Tk()
    ent=Entry(a)
    ent.pack()
    tovar= ent.get()
    def check():    
        fptr.setParam(IFptr.LIBFPTR_PARAM_COMMODITY_NAME, tovar )
        fptr.setParam(IFptr.LIBFPTR_PARAM_PRICE, 100)
        fptr.setParam(IFptr.LIBFPTR_PARAM_QUANTITY, 5)
        fptr.setParam(IFptr.LIBFPTR_PARAM_TAX_TYPE, IFptr.LIBFPTR_TAX_VAT10)
        fptr.setParam(IFptr.LIBFPTR_PARAM_TAX_SUM, 51.5)
        fptr.registration()
        print(fptr.errorDescription())
    button6 = Button(a, text='чек', width=10, height=5, bg='green', fg='Black', font='arial14',command=check)
    button6.pack()   
    a.mainloop()
    
root = Tk()
button4 = Button(root, text='параметры чека', width=10, height=5, bg='green', fg='Black', font='arial14', command=enter)
button4.pack()
button4 = Button(root, text='х отчет', width=10, height=5, bg='green', fg='Black', font='arial14', command=d)
button4.pack()
button1 = Button(root, text='порт', width=10, height=5, bg='green', fg='Black', font='arial14', command=a)
button1.pack()
button2 = Button(root, text='информация', width=10, height=5, bg='green', fg='Black', font='arial14', command=e)
button2.pack()
button3 = Button(root, text='статус соединения', width=15, height=5, bg='white', fg='Black', font='arial14',command=b)
button3.pack()
com = Entry(root)
com.pack()
root.mainloop()
