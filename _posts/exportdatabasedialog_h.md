#ifndef EXPORTDATABASEDIALOG_H
#define EXPORTDATABASEDIALOG_H

#include <QDialog>
#include <QLineEdit>

class ExportDatabaseDialog : public QDialog {
    Q_OBJECT

public:
    explicit ExportDatabaseDialog(QWidget *parent = nullptr);
    QString getFilePath() const;

private slots:
    void chooseFile();

private:
    QLineEdit *pathEdit; [exportdatabasedialog.cpp](..\Qtpro\Testbat\exportdatabasedialog.cpp) 
};

#endif // EXPORTDATABASEDIALOG_H