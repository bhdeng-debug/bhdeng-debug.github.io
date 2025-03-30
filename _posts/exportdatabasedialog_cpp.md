#include "ExportDatabaseDialog.h"
#include <QPushButton>
#include <QVBoxLayout>
#include <QHBoxLayout>
#include <QFileDialog>
#include <QLabel>
#include <QDateTime>
#include <QCoreApplication>

ExportDatabaseDialog::ExportDatabaseDialog(QWidget *parent) : QDialog(parent) {
    setWindowTitle("导出数据库");
    setFixedSize(800, 300);

    // 文件路径输入框和选择按钮
    QHBoxLayout *pathLayout = new QHBoxLayout;
    QLabel *label = new QLabel("导出路径:", this);
    
    QString filePath = "";
    QString dateTime = QDateTime::currentDateTime().toString("yyyyMMdd_HHmmss");
    
    // 设置默认文件名为 cdb_加日期时间
    if (filePath.isEmpty()) {
        filePath = "cdb_" + dateTime + ".sql";
    }
    filePath = "C:/Users/Public/" + filePath;
    pathEdit = new QLineEdit(filePath,this);
    QPushButton *browseButton = new QPushButton("浏览...", this);
    pathLayout->addWidget(label);
    pathLayout->addWidget(pathEdit);
    pathLayout->addWidget(browseButton);
    
    // 确定和取消按钮
    QHBoxLayout *buttonLayout = new QHBoxLayout;
    QPushButton *okButton = new QPushButton("确定", this);
    QPushButton *cancelButton = new QPushButton("取消", this);
    buttonLayout->addStretch();
    buttonLayout->addWidget(okButton);
    buttonLayout->addWidget(cancelButton);
    
    // 总体布局
    QVBoxLayout *mainLayout = new QVBoxLayout(this);
    mainLayout->addLayout(pathLayout);
    mainLayout->addLayout(buttonLayout);
    
    // 连接信号槽
    connect(browseButton, &QPushButton::clicked, this, &ExportDatabaseDialog::chooseFile);
    connect(okButton, &QPushButton::clicked, this, &QDialog::accept);
    connect(cancelButton, &QPushButton::clicked, this, &QDialog::reject);
}

QString ExportDatabaseDialog::getFilePath() const {
    return pathEdit->text();
}

void ExportDatabaseDialog::chooseFile() {

    QString filePath =QCoreApplication::applicationDirPath();
    QString dateTime = QDateTime::currentDateTime().toString("yyyyMMdd_HHmmss");
    
    // 设置默认文件名为 cdb_加日期时间
    if (filePath.isEmpty()) {
        filePath += "cdb_" + dateTime + ".sql";
    }
    filePath = QFileDialog::getSaveFileName(this, "选择导出路径", filePath, "数据库文件 (*.sql)");
    
    if (!filePath.isEmpty()) {
        // 获取当前日期和时间
        QString dateTime = QDateTime::currentDateTime().toString("yyyyMMdd_HHmmss");
    
        // 设置默认文件名为 cdb_加日期时间
        if (filePath.isEmpty()) {
            filePath = "cdb_" + dateTime + ".sql";
        }
    
        // 将文件路径显示在编辑框中
        pathEdit->setText(filePath);
    }
    // QString filePath = QFileDialog::getSaveFileName(this, "选择导出路径", "", "数据库文件 (*.sql)");
    // if (!filePath.isEmpty()) {
    //     pathEdit->setText(filePath);
    // }
}
