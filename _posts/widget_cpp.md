#include "widget.h"
#include "ui_widget.h"
#include "exportdatabasedialog.h"
#include <QPushButton>
#include <QMessageBox>
#include <QDebug>
#include <QProcess>
#include <QDir>

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    QPushButton *pushButton_save = new QPushButton("日志导出",this);

    connect(pushButton_save,&QPushButton::clicked,this,&slots_saveLog);
}

Widget::~Widget()
{
    delete ui;
}

void Widget::slots_saveLog()
{
    ExportDatabaseDialog dialog;
    if (dialog.exec() == QDialog::Accepted) {
        QString exportPath = dialog.getFilePath();
        // exportPath = QDir::toNativeSeparators("C:/Users/Public/111.sql");
        if (!exportPath.isEmpty()) {

            qDebug() << QStringLiteral("数据库导出到:") << exportPath;
    
            // 构造命令
            QString batFile = QCoreApplication::applicationDirPath() + "/export_db.bat";
    
            if (!QFile::exists(batFile)) {
                qDebug() << "错误: 找不到 export_db.bat 文件:" << batFile;
                return;
            }
            QStringList arguments;
            arguments << exportPath;
    
            QProcess process;
    
            process.startDetached(batFile, arguments);


        }
    }
}
