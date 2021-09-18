# memory-
math problems

from PyQt5.QtCore import Qt
from PyQt5.QtWidgets import QApplication, QWidget, QPushButton, QLabel, QVBoxLayout, QHBoxLayout, QGroupBox, QRadioButton,  QButtonGroup
from random import shuffle
app = QApplication([])
main_win = QWidget()

main_win.setWindowTitle('Memory Card')

main_win.resize(200, 200)

okb = QPushButton("Ответить")

main_win.cur_quest = -1

main_win.score = 0

questu = QLabel('1')

qBox =  QGroupBox("Варианты ответов")

b1 = QRadioButton("1")
b2 = QRadioButton("1")
b3 = QRadioButton("1")
b4 = QRadioButton("1")

lay1 = QHBoxLayout()
lay2 = QVBoxLayout()
lay3 = QVBoxLayout()

lay2.addWidget(b1)
lay2.addWidget(b2)
lay3.addWidget(b3)
lay3.addWidget(b4)

lay1.addLayout(lay2)
lay1.addLayout(lay3)
qBox.setLayout(lay1)

box = QButtonGroup() 

box.addButton(b1)
box.addButton(b2)
box.addButton(b3)
box.addButton(b4)

card_layout = QVBoxLayout()

line1_as = QHBoxLayout()
line2_as = QHBoxLayout()

ansBox =  QGroupBox("Результат теста")

text1 = QLabel("Ответ:")
text2 = QLabel("ответ будет тут!")

line1_as.addWidget(text1, alignment = Qt.AlignHCenter)
line2_as.addWidget(text2, alignment =  Qt.AlignHCenter)

card_layout.addLayout(line1_as)
card_layout.addLayout(line2_as)

ansBox.setLayout(card_layout)

main_line = QVBoxLayout()
line1_q = QHBoxLayout()
line2_q = QHBoxLayout()
line3_q = QHBoxLayout()

line1_q.addWidget(questu)
line2_q.addWidget(qBox)
line3_q.addWidget(okb)

main_line.addLayout(line1_q)
main_line.addLayout(line2_q)
main_line.addLayout(line3_q)

line2_q.addWidget(ansBox)

ansBox.hide()

def show_result():
    qBox.hide()
    ansBox.show()
    okb.setText('Следующий вопрос')

def show_question():
    qBox.show()
    ansBox.hide()

    okb.setText('Ответить')

    box.setExclusive(False)

    b1.setChecked(False)
    b2.setChecked(False)
    b3.setChecked(False)
    b4.setChecked(False)

    box.setExclusive(True)

answers = [b1, b2, b3, b4]

class Question():
    def __init__(self, question, right, wrong1, wrong2, wrong3):
        self.question = question
        self.right = right
        self.wrong1 = wrong1
        self.wrong2 = wrong2
        self.wrong3 = wrong3

def ask(q):
    shuffle(answers)

    answers[0].setText(q.right)
    answers[1].setText(q.wrong1)
    answers[2].setText(q.wrong2)
    answers[3].setText(q.wrong3)

    questu.setText(q.question)


def check1():
    if answers[0].isChecked(): 


        text2.setText("Правильный")
        
        main_win.score +=1
        show_result()

    else:
       text2.setText("Неправильный") 
       show_result()

def test():
    if 'Ответить' == okb.text():
        
        show_result()
        check1()

    else:
        next_quest()
        show_question()
def scor():
    print("Статистика:")
    print("Всего вопросов:", i) 
    print("Правильных ответов:", main_win.score)
    print("Процент правильных ответов:", str(int(main_win.score)/i * 100) +"%")

def next_quest():
    main_win.cur_quest += 1
    if len(ask1) == main_win.cur_quest:
        main_win.cur_quest = -1
        main_win.hide()
        scor() 
    ask(ask1[main_win.cur_quest])
ask1 = []

ask1.append(Question("Сколько будет 2 + 2?", "4", '22', '3', '2'))
ask1.append(Question("Выберите лишнее: 2, 4, 5, 8?", '5', '2', '4', '8'))
ask1.append(Question("сколько будет 4!", "24", '10', '20', '4'))
ask1.append(Question("Сколько будет 3! + 4!", '30', '34', '7', '11'))
ask1.append(Question("Сколько будет 3 * 4?", '12', '32', '7', '30'))

shuffle(ask1)

i = len(ask1)

next_quest()

okb.clicked.connect(test)

main_win.setLayout(main_line)
main_win.show()
app.exec_()



