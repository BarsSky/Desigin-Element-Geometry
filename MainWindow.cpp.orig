#include "MainWindow.h"
#include "ui_MainWindow.h"
#include <QFileDialog>
#include <QMessageBox>
#include <algorithm>
#include "QScrollBar"
#include "math.h"
#include "DialogButtonBottom.h"
#include "Graphics_view_zoom.h"
#include "SettingsDialog.h"
#include "UniversalDialog.h"
#include "Dlg3DParametrs.h"
#include "QClipboard"


void MainWindow::keyOnRotate(int key, int ID)
{
    switch (key)
    {
    case TURNRIGHT:
        //        if(rotateElementOnCentr){
        //            rotateAngle = rotateOnAxie;
        rotateAngle += keyStep;
        if (rotateAngle >= 360)
            rotateAngle = 0;
        //        }
        //        else{
        //            rotateAngle = rotateOnCenter;
        //            rotateAngle += keyStep;
        //            if (rotateAngle >= 360)
        //                rotateAngle = 0;
        //        }

        break;
    case TURNLEFT:
        //if(rotateElementOnCentr){
        //rotateAngle = rotateOnAxie;
        rotateAngle -= keyStep;
        if (rotateAngle < 0)
            rotateAngle = 359;
        //        }
        //        else{
        //            rotateAngle = rotateOnCenter;
        //            rotateAngle -= keyStep;
        //            if (rotateAngle < 0)
        //                rotateAngle = 359;
        //        }
        break;
    default:
        break;
    }
    CanChange = false;
    if (!changePositionAndRotationEl)
    {
        float rotateMod = m_itemsListBuff.at(ID - 1).getRotateAngle() + ((key == TURNLEFT) ? -keyStep : keyStep);
        m_itemsListBuff.at(ID - 1).setRotateAngle(rotateMod);
        m_itemsListBuff.at(ID - 1).graphicsItem()->setRotation(m_itemsListBuff.at(ID - 1).getRotateAngle());
        //RotationElements(x_PosEl, y_PosEl, xLast, yLast, rotateAngle);
        ui->lE_Rotation->setText(QString::number(rotateAngle));
        //Запоминаем текущий шаг
        markedChangedItem(ID);
        if (addedElements)
            realTimeRotation(ID, rotateAngle);//((key == TURNLEFT) ? -keyStep : keyStep));
    }
    else
    {
        //вращение каждого элемента вокруг своей оси

        if (m_changePositionRotation.size() > 0)
        {

            double new_centerX = 0., new_centerY = 0.;
            for (unsigned int i = 0; i < m_changePositionRotation.size(); i++)
            {
                new_centerX += x_Pos.at(m_changePositionRotation.at(i).Id());
                new_centerY += y_Pos.at(m_changePositionRotation.at(i).Id());
            }
            new_centerX /= m_changePositionRotation.size();
            new_centerY /= m_changePositionRotation.size();
            //необходимо предварительно отцентрировать выбранные координаты

            for (unsigned int i = 0; i < m_changePositionRotation.size(); i++)
            {
                if (rotateElementsOnCentr)//если необходимо повернуть вокруг общего центра
                {
                    double angle = ((key == TURNLEFT) ? keyStep : -keyStep);//m_changePositionRotation.at(i).getRotateAngle() +

                    double stepX = ((x_Pos.at(m_changePositionRotation.at(i).Id()) - new_centerX)* cos(angle * PI / 180)
                                    - (y_Pos.at(m_changePositionRotation.at(i).Id()) - new_centerY) * sin(angle * PI / 180));//

                    double stepY = ((x_Pos.at(m_changePositionRotation.at(i).Id()) - new_centerX)* sin(angle * PI / 180)
                                    + (y_Pos.at(m_changePositionRotation.at(i).Id()) - new_centerY)* cos(angle * PI / 180));


                    x_Pos.at(m_changePositionRotation.at(i).Id()) = new_centerX + stepX;
                    y_Pos.at(m_changePositionRotation.at(i).Id()) = new_centerY + stepY;

                    ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_X,
                                                new QTableWidgetItem(QString("%1").arg(x_Pos.at(m_changePositionRotation.at(i).Id()))));
                    ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_Y,
                                                new QTableWidgetItem(QString("%1").arg(y_Pos.at(m_changePositionRotation.at(i).Id()))));


                    m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setX(x_Pos.at(m_changePositionRotation.at(i).Id()) / static_cast<double>(scaleW) + gViewModule->width()/2); // static_cast<double>(scaleW)
                    // + static_cast<double>(deltW / scaleW));
                    m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setY(-y_Pos.at(m_changePositionRotation.at(i).Id())  / static_cast<double>(scaleH) + gViewModule->height()/2); // static_cast<double>(scaleH)
                    // + static_cast<double>(deltH / scaleH));
                    //необходимо считать угол относительно которого осуществлять поворот
                    m_changePositionRotation.at(i).setRotateAngle(m_changePositionRotation.at(i).getRotateAngle() + angle);
                    m_changePositionRotation.at(i).graphicsItem()->setPos(m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).X(), m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).Y());
                    m_changePositionRotation.at(i).setX(m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).X());
                    m_changePositionRotation.at(i).setY(m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).Y());

                    if (addedElements)
                    {
                        realTimeRotation(m_changePositionRotation.at(i).Id() + 1, rotateAngle);// -angle);//
                    }
                }
                else
                {
                    m_changePositionRotation.at(i).setRotateAngle(rotateAngle);//m_changePositionRotation.at(i).getRotateAngle() +

                    if (addedElements)
                        realTimeRotation(m_changePositionRotation.at(i).Id() + 1, rotateAngle);// - m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).getRotateAngle());
                }
                //rotateAngle = m_changePositionRotation.at(i).getRotateAngle() + ((key == TURNLEFT) ? 1 : -1);

                //осуществляем перемещение для всех элементов массива
                m_changePositionRotation.at(i).graphicsItem()->setRotation(rotateAngle);
                //поворачиваем на нужный угол
                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setRotateAngle(rotateAngle);

                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setX(m_changePositionRotation.at(i).X());
                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setY(m_changePositionRotation.at(i).Y());

                markedChangedItem(m_changePositionRotation.at(i).Id());
            }
        }
        ui->lE_Rotation->setText(QString::number(rotateAngle));
    }
    CanChange = true;
    //запоминаем изменения углов
    if(rotateElemCentr)
        rotateOnAxie = rotateAngle;
    else
        rotateOnCenter = rotateAngle;
}

void MainWindow::RotationElements(std::vector<double> xcoord, std::vector<double> ycoord, std::vector<double> &xNew, std::vector<double> &yNew, float angle)
{
    //данный алгоритм вращает элементы вокруг центра всей апертуры
    for (unsigned int i = 0; i < xcoord.size(); i++)
    {
        xNew.at(i) = (xcoord[i] * cos(angle*PI / 180) - ycoord[i] * sin(angle*PI / 180));
        yNew.at(i) = (xcoord[i] * sin(angle*PI / 180) + ycoord[i] * cos(angle*PI / 180));
    }
    //сохраняем координаты после поворота
}

void MainWindow::calcModulsCoords(float X, float Y, int y)
{
    for (unsigned int i = 0; i < xLast.size(); i++)
    {
        xLast.at(i) = xLast.at(i) + X;//scaleW;
        yLast.at(i) = yLast.at(i) + Y;///scaleH;
    }
}


Qt3DCore::QEntity *MainWindow::createScene()
{
    // Root entity
    Qt3DCore::QEntity *rootEntity = new Qt3DCore::QEntity;

    // Material
    Qt3DRender::QMaterial *material = new Qt3DExtras::QPhongMaterial(rootEntity);

    // Torus
    Qt3DCore::QEntity *torusEntity = new Qt3DCore::QEntity(rootEntity);
    Qt3DExtras::QTorusMesh *torusMesh = new Qt3DExtras::QTorusMesh;
    torusMesh->setRadius(5);
    torusMesh->setMinorRadius(1);
    torusMesh->setRings(100);
    torusMesh->setSlices(20);

    // Создадим параллелепипед имитирующий элемент АР
    // Cuboid shape data

    Qt3DCore::QTransform *torusTransform = new Qt3DCore::QTransform;
    torusTransform->setScale3D(QVector3D(1.5, 1, 0.5));
    torusTransform->setRotation(QQuaternion::fromAxisAndAngle(QVector3D(1, 0, 0), 45.0f));
    torusEntity->addComponent(torusMesh);
    torusEntity->addComponent(torusTransform);
    torusEntity->addComponent(material);

    // Sphere
    Qt3DCore::QEntity *sphereEntity = new Qt3DCore::QEntity(rootEntity);
    Qt3DExtras::QSphereMesh *sphereMesh = new Qt3DExtras::QSphereMesh;
    sphereMesh->setRadius(3);

    Qt3DCore::QTransform *sphereTransform = new Qt3DCore::QTransform;

    OrbitTransformController *controller = new OrbitTransformController(sphereTransform);
    controller->setTarget(sphereTransform);
    controller->setRadius(20.0f);

    QPropertyAnimation *sphereRotateTransformAnimation = new QPropertyAnimation(sphereTransform);
    sphereRotateTransformAnimation->setTargetObject(controller);
    sphereRotateTransformAnimation->setPropertyName("angle");
    sphereRotateTransformAnimation->setStartValue(QVariant::fromValue(0));
    sphereRotateTransformAnimation->setEndValue(QVariant::fromValue(360));
    sphereRotateTransformAnimation->setDuration(10000);
    sphereRotateTransformAnimation->setLoopCount(-1);
    sphereRotateTransformAnimation->start();

    sphereEntity->addComponent(sphereMesh);
    sphereEntity->addComponent(sphereTransform);
    sphereEntity->addComponent(material);

    return rootEntity;
}

Qt3DCore::QEntity *MainWindow::createScene(std::vector<double> x_s, std::vector<double> y_s, std::vector<double> z_s)
{
    Qt3DCore::QEntity *rootEntity = new Qt3DCore::QEntity;

    Qt3DCore::QTransform *objectTransform = new Qt3DCore::QTransform(rootEntity);
    // Работа со светом и материалом
    Qt3DRender::QPointLight * light = new Qt3DRender::QPointLight();
    //Qt3DRender::QMaterial *material = new Qt3DExtras::QPhongMaterial(rootEntity);
    Qt3DCore::QEntity *lightEntity = new Qt3DCore::QEntity(rootEntity);
    Qt3DCore::QTransform *lightTransform = new Qt3DCore::QTransform();

    light->setIntensity(1.);
    light->setColor(Qt::white);
    lightTransform->setTranslation(QVector3D(0, 0, 3000.f));
    lightEntity->addComponent(light);
    lightEntity->addComponent(lightTransform);
    m_objList.clear();

    // CuboidMesh Transform
    int count = 0;
    for(unsigned int j = 0; j < m_objects.size(); j ++)
        for (int i = 0; i < m_objects.at(j).numElem(); i++)
        {
            //необходимо учитывать что координата z в отображении 3D расположена в основании объекта
            //соответственно при учете высоты блока нам необходимо перенести координату Z в крышку блока
            //задаем расположение
            obj->SetPosition(QVector3D(static_cast<float>(x_s.at(count)),
                                       static_cast<float>(y_s.at(count)),
                                       static_cast<float>(((z_s.at(count))) - zHeight)));//нормируем положение блока
            obj->SetTypeObj(m_objects.at(j).type());
            //задаем цвет элемента
            obj->SetColor(m_objects.at(j).color());
            //задаем размеры элемента
            switch (m_objects.at(j).type()) {
            case RAMP:
                obj->SetExtent(static_cast<float>(m_objects.at(j).width()), static_cast<float>(m_objects.at(j).height()),  static_cast<float>(zHeight));
                if (dlRotate)
                    obj->SetRotationObj(0.f, 0.f, rotationArray.at(count));//заенить на высоту и ширин эллипса или реализовать размеры при запросе конфигуратора
                break;
            case CIRCLE:
                //для кооректного отображения необходимо повернуть цилиндры
                obj->SetRotationObj(90.f, 0.f, 0.f);//заенить на высоту и ширин эллипса или реализовать размеры при запросе конфигуратора
                //TODO:
                obj->SetExtent(static_cast<float>(m_objects.at(j).height()/2), static_cast<float>(m_objects.at(j).height()/2), static_cast<float>(zHeight));
                break;
            case POLYGON:
                //устанавливаем количество сторон
                obj->sideNumber = m_objects.at(j).side();
                //для кооректного отображения необходимо повернуть цилиндры
                obj->SetRotationObj(90.f, 0.f, 0.f);//заенить на высоту и ширин эллипса или реализовать размеры при запросе конфигуратора

                obj->SetExtent(static_cast<float>(m_objects.at(j).width()/2), static_cast<float>(m_objects.at(j).side()), static_cast<float>(zHeight));
                break;
            }
            //Cuboid инициализируем объект сцены
            m_cuboidEntity = new Qt3DCore::QEntity;
            m_cuboidEntity = obj->init(rootEntity);
            //запоминаем объект
            m_objList.push_back(obj_List(static_cast<int>(count), m_cuboidEntity, objectTransform));
            count++;
        }
    //после формирования 3d отображаем 2d
    //    quickScene = new Qt3DRender::Quick::QScene2D(rootEntity);
    //    quickScene->setMouseEnabled(true);
    //    itemQ = new quickItemClass();
    //    itemQ->setSize(QSizeF(100.,100.));
    //    quickScene->setItem(itemQ);


    return rootEntity;
}

void MainWindow::freeMemory()
{
    x_Pos.clear();
    y_Pos.clear();
    z_Pos.clear();
    rotationArray.clear();
    rotateAngle = 0;
    if (m_itemsListM.size() != 0)
        for (unsigned int i = 0; i < m_itemsListM.size(); i++)
            sceneM->removeItem(m_itemsListM.at(i).graphicsItem());
    sceneM->clear();
    m_itemsListM.clear();
    if (m_itemsListE.size() != 0)
        for (unsigned int i = 0; i < m_itemsListE.size(); i++)
            sceneE->removeItem(m_itemsListE.at(i).graphicsItem());
    sceneE->clear();
    m_itemsListE.clear();
    m_changePositionRotation.clear();
    rendo_changePositionRotation.clear();
    addedElements = false;
    changePositionAndRotationEl = false;
    emit(disableCheck());
}

void MainWindow::createStepMemory(std::vector<UndoRendo> &buff)
{
    buff.emplace_back(UndoRendo(m_itemsListBuff));//запоминаем начальное состояние системы

    //проверяем текущее значение шага
    //если меньше размера то заплоняем
    // 	if (currentStep != m_step.size())
    // 	{
    // 		//необходимо сдвинуть все последующие значения в векторе на 1
    // 		auto iter = m_step.cbegin();//задаем начальное положение
    // 		//помещаем массив на новое место
    // 		m_step.emplace(iter + currentStep, UndoRendo(m_itemsListBuff));
    // 		//stepBack = 1;
    // 	}
    // 	else
    // 	{
    // 		m_step.emplace_back(UndoRendo(m_itemsListBuff));
    // 		stepBack = 2;
    // 	}
    // 	currentStep++;
}

void MainWindow::markedChangedItem(int ID)
{
    //для выделенной группы элементов необходимо использовать другой тип массива
    // 	if (changePositionAndRotationEl)
    // 	{
    // 		rendo_changePositionRotation.emplace_back(m_changePositionRotation);
    // 		//if (rendo_changePositionRotation.size() > 1)
    // 		currentStep++;
    // 	}
    // 	else
    // 	{
    // 		if (changedItem.size() != 0)
    // 			if (currentStep != changedItem.size() - 1)//необходимо добавить новое действие в конец массива а все что было между перевернуть
    // 			{
    // 				std::vector<stepMemory> buffVec;
    // 				//auto iter = changedItem.cbegin();//задаем начальное положение
    // 				//changedItem.emplace(iter + currentStep, m_itemsListBuff.at(ID));
    // 				for (unsigned int k = currentStep; k < changedItem.size(); k++)
    // 				{
    // 					if (changedItem.size() - k > k)
    // 					{
    // 						buffVec.emplace_back(changedItem.at(k));
    // 						changedItem.at(k) = changedItem.at(changedItem.size() - k);
    // 					}
    // 					else
    // 						changedItem.at(k) = buffVec.back();// (changedItem.size() - 1 - k);
    // 				}
    // 				buffVec.clear();
    // 				currentStep = changedItem.size();
    // 				//changedItem.emplace_back(m_itemsListBuff.at(ID));
    // 			}
    // 		changedItem.emplace_back(m_itemsListBuff.at(ID));
    //
    // 		//if (changedItem.size() > 1)
    // 		currentStep++;
    // 	}
}

bool MainWindow::createElementsDraw(QGraphicsView * view, QString fileName, QGraphicsScene* sceneV, std::vector<List> &itemsGraph)
{
    //формируем новую сцену
    sceneV->setSceneRect(-view->x(), -view->y(), view->width(), view->height());
    view->setScene(sceneV);
    view->setRenderHint(QPainter::Antialiasing, true);    /// Устанавливаем сглаживание
    //LoadCoordsGview(fileName);
    //вызываем диалог для корректировки размеров модуля
    diagMod = new DialogMod();
    diagMod->initial(areYouOnDarkSide);
    if (!diagMod->exec())
        return false;
    diagIndex = diagMod->m_index;
    if(loadSubElements)
        dDiagIndex = diagIndex;
    dlgColor = diagMod->elementColor();
    //после успешного завершения диалога принимаем тип данных
    switch (diagIndex) {
    case RAMP:
        widthModule = diagMod->ModW;
        heightModule = diagMod->ModH;
        elm_Distance = diagMod->m_elDistant;
        break;
    case POLYGON:
        widthModule = heightModule = 2 * diagMod->WidthSide;
        widthSide = diagMod->WidthSide;
        NumSide = diagMod->NumOfSides;
        elm_Distance = diagMod->m_elDistant;
        break;
    case CIRCLE:
        widthModule = heightModule = 2 * diagMod->elRad;
        radiusModule = diagMod->elRad;
        NumSide = diagMod->NumOfSides;
        elm_Distance = diagMod->m_elDistant;
        break;
    }

    LoadCoordsGview(fileName);

    inFile = true;
    //отрисовываем объекты
    drawInGraphicScene(fileXe, fileYe, fileZe, view, sceneV, itemsGraph);
    return true;
}

void MainWindow::corectVisualPicture()
{
    MinMaxCoordsCalc(x_Pos, y_Pos);

    double SquareW = std::abs(maxX - minX);//крайнее левое положение координат по X
    double SquareH = std::abs(maxY - minY);//крайнее левое положение координат по Y

    double gVwidth = gViewModule->width() / 2;
    double gVheight = gViewModule->height() / 2;

    double coeff = (gViewModule->width()*gViewModule->height()) / (SquareH*SquareW);
    scaleH = scaleW = static_cast<float>(coeff);

    for (unsigned int i = 0; i < x_Pos.size(); i++)
    {
        x_Pos.at(i) = (x_Pos.at(i) - static_cast<double>(SquareW / 2));// +static_cast<double>(gVwidth));//уходим от центрирования координат для отображения объектов в виджете
        y_Pos.at(i) = (y_Pos.at(i) - static_cast<double>(SquareH / 2));// +static_cast<double>(gVheight));
    }
}

void MainWindow::createElementPaint(int Id, QRect rectP, QRectF rectB, bool visualMark, int index)
{
    paintAr = new PaintAperture(Id + 1, rectP, rectB, setVisibleNum->isChecked(), setVisibleNumFastening->isChecked(),dlgColor);//chouseColor(countColor)
    //учитываем количество сторон многоугольника
    if (index == POLYGON)
        paintAr->SetNumSide(NumSide);
    paintAr->setFlags(QGraphicsItem::ItemIsMovable);
    //коннектим его с отображением номера
    connect(setVisibleNum, SIGNAL(triggered(bool)), paintAr, SLOT(VisibleNumber(bool)));
    //коннектим его с отображением креплений
    connect(setVisibleNumFastening, SIGNAL(triggered(bool)), paintAr, SLOT(VisibleFastening(bool)));
    //коннектим его с слотом слежения координаты его положения
    connect(paintAr, SIGNAL(addValueTable(int)), this, SLOT(addValueMouse(int)));
    //коннект на перемещение объекта посредством клавиш
    connect(paintAr, SIGNAL(checkedItem(int, bool, bool)), this, SLOT(addNumberItem(int, bool, bool)));
    //сигнал на смену цвета
    connect(this, SIGNAL(disableCheck()), paintAr, SLOT(VisualMod()));
    //соединяем с сигналом от мыши
    connect(paintAr, SIGNAL(ifPressItemPos(QPointF, int)), this, SLOT(changeMousePositionForElements(QPointF, int)));
    //выставляем индекс
    paintAr->SetIndex(index);
}

void MainWindow::Undo()
{
    if (currentStep > 1)
    {
        if (changedItem.size() > 0)
        {
            //полностью заменяем текущие значения на сохраненные
            m_itemsListBuff.at(changedItem.at(currentStep - 1).changedList().Id()).graphicsItem()->setPos(changedItem.at(currentStep - 1).changedList().X(),
                                                                                                          changedItem.at(currentStep - 1).changedList().Y());
            m_itemsListBuff.at(changedItem.at(currentStep - 1).changedList().Id()).graphicsItem()->setRotation(changedItem.at(currentStep - 1).changedList().getRotateAngle());

            creatTable(changedItem.at(currentStep - 1).changedList().X(), changedItem.at(currentStep - 1).changedList().Y(), changedItem.at(currentStep - 1).changedList().Z(), changedItem.at(currentStep - 1).changedList().Id());
            // 				for (unsigned int j = 0; j < m_step.at(currentStep- stepBack).takeStep().size(); j++)
            // 				{
            // 					m_itemsListBuff.at(j).graphicsItem()->setPos(m_step.at(currentStep - stepBack).takeStep().at(j).X(), m_step.at(currentStep - stepBack).takeStep().at(j).Y());
            // 					m_itemsListBuff.at(j).graphicsItem()->setRotation(m_step.at(currentStep - stepBack).takeStep().at(j).getRotateAngle());
            // 					//необходимо перезаполнять таблицу
            // 					//
            // 				}
            currentStep -= 1;
        }
    }
    else
    {
        for (unsigned int b = 0; b < m_itemsListBuff.size(); b++)
        {
            m_itemsListBuff.at(b).graphicsItem()->setPos(m_stepBuff.at(0).takeStep().at(b).X(),
                                                         m_stepBuff.at(0).takeStep().at(b).Y());
            m_itemsListBuff.at(b).graphicsItem()->setRotation(m_stepBuff.at(0).takeStep().at(b).getRotateAngle());

            creatTable(m_stepBuff.at(0).takeStep().at(b).X(),
                       m_stepBuff.at(0).takeStep().at(b).Y(),
                       m_stepBuff.at(0).takeStep().at(b).Z(),
                       b);
        }
        m_itemsListBuff = m_stepBuff.at(0).takeStep();
        QMessageBox::warning(container, "Предупреждение", "Достигнуто начальное состояние элементов");
        changedItem.clear();
        currentStep = 0;
    }
}

void MainWindow::Rendo()
{
    if (currentStep < changedItem.size() - 1)
    {
        if (changedItem.size() > 0)
        {
            //полностью заменяем текущие значения на сохраненные
            m_itemsListBuff.at(changedItem.at(currentStep + 1).changedList().Id()).graphicsItem()->setPos(changedItem.at(currentStep + 1).changedList().X(),
                                                                                                          changedItem.at(currentStep + 1).changedList().Y());
            m_itemsListBuff.at(changedItem.at(currentStep + 1).changedList().Id()).graphicsItem()->setRotation(changedItem.at(currentStep + 1).changedList().getRotateAngle());

            creatTable(changedItem.at(currentStep + 1).changedList().X(), changedItem.at(currentStep + 1).changedList().Y(), changedItem.at(currentStep + 1).changedList().Z(), changedItem.at(currentStep + 1).changedList().Id());
            // 				for (unsigned int j = 0; j < m_step.at(currentStep- stepBack).takeStep().size(); j++)
            // 				{
            // 					m_itemsListBuff.at(j).graphicsItem()->setPos(m_step.at(currentStep - stepBack).takeStep().at(j).X(), m_step.at(currentStep - stepBack).takeStep().at(j).Y());
            // 					m_itemsListBuff.at(j).graphicsItem()->setRotation(m_step.at(currentStep - stepBack).takeStep().at(j).getRotateAngle());
            // 					//необходимо перезаполнять таблицу
            // 					//
            // 				}
            currentStep += 1;
        }
    }
}

QColor MainWindow::chouseColor(int count)
{
    QColor color;
    switch(count){
    case 1:
        color = QColor(Qt::red);
        break;
    case 2:
        color = QColor(Qt::darkBlue);
        break;
    case 3:
        color = QColor(Qt::darkMagenta);
        break;
    case 4:
        color = QColor(Qt::white);
        break;
    default:
        color = QColor(Qt::darkGreen);
        break;
    }
    return color;
}

void MainWindow::drawBackGroundGrid()
{
    gViewModule->setBackgroundBrush(colorList->color());
    //gViewElements->setBackgroundBrush(QBrush(Qt::black));
}

void MainWindow::startLabel()
{
    firstLabel = new QLabel();
    firstLabel->setSizePolicy(QSizePolicy::Expanding,QSizePolicy::Expanding);
    QFont font = QFont("Segoe UI", 20);
    firstLabel->setAlignment(Qt::AlignHCenter|Qt::AlignVCenter);
    firstLabel->setFont(font);
    firstLabel->setStyleSheet("color: #787878;");
    firstLabel->setText("Для начала работы выберите файл или нажмите кнопку рассчитать");
    ui->verticalLayout_2->addWidget(firstLabel);
}

void MainWindow::sceneParametrs()
{
    std::vector<bool> b_param;
    std::vector<double> d_param;
    //
    b_param.emplace_back(autoCalculate);
    b_param.emplace_back(dlgIsOpen);
    //
    d_param.emplace_back(zHeight);

    Dlg3DParametrs* d3D = new Dlg3DParametrs(this);
    d3D->dlgInit(b_param,d_param);
    if(!d3D->exec())
        return;
    dlgIsOpen = d3D->dlgIsOpen;
    autoCalculate = d3D->autoCalculate;
    if(!autoCalculate)
        zHeight = d3D->zHeight;
    QSettings geomMasterSettings(ORGANIZATION_NAME, APPLICATION_NAME);
    geomMasterSettings.setValue(OPEN3DDLG,dlgIsOpen);
    geomMasterSettings.setValue(AUTOCALC,autoCalculate);
}

void MainWindow::create3DWSceneText()
{
    Qt3DCore::QTransform* objTransform = new Qt3DCore::QTransform();
    objTransform->setTranslation(QVector3D(0,0,100));
    objTransform->setScale(1.0f);

    QFont font = QFont("monospace",14);
    objText2D = new Qt3DExtras::QText2DEntity();//scene
    objText2D->setFont(font);
    objText2D->setWidth(1000);
    objText2D->setHeight(1000);
    objText2D->setColor(Qt::black);
    objText2D->setText("SomeText");
    objText2D->addComponent(objTransform);
}

void MainWindow::create3DSceneParameters()
{
    ui->statusBar->showMessage("формирование 3D сцены",-1);
    if(!dlgIsOpen)
        sceneParametrs();
    if(autoCalculate)
    {
        if(widthModule>heightModule)
            zHeight = widthModule;
        else
            zHeight = heightModule;
    }

    mod3DOn = true; //даем разрешение на отображение 3D окна
    ui->tabWidget->setVisible(false);		// For camera controls
    //очищаем сцену
    if(scene->components().size()>0)
    {
        for(int i = 0; i < scene->components().size(); i++)
            scene->removeComponent(scene->components().at(i));
        delete scene;
        delete camera;
        delete camController;
    }
    //информация на передний план
    //create3DWSceneText();
    ui->statusBar->showMessage("задаем параметры камеры",-1);
    create3DCamera();

    //передаем сформированную сцену на отображение
    view->setRootEntity(scene);
    //view->setMouseGrabEnabled(true);
    //view->setRootEntity(objText2D);
    ui->statusBar->showMessage("Вывод 3D изображения",-1);
    view->show();
}

void MainWindow::create3DCamera()
{
    float maxic = 0;
    ui->statusBar->showMessage("Обработка 3D объектов",-1);
    //проверка на возможность формирования сцены
    if(x_Pos.size() == 0)
    {
        scene = createScene();
        maxic = 30;
    }
    else
    {
        //производим формирование сцены при нажатии чекбокса
        scene = createScene(x_Pos, y_Pos, z_Pos);
        MinMaxCoordsCalc(x_Pos,y_Pos);
        maxic = sqrt(pow(maxX,2) + pow(maxY,2))/1.;// 3.f * pow(abs(maxX)*abs(maxY),2)/abs(maxX)*abs(maxY);
    }
    double aspectRatio = 0.;
    //расчитываем соотношение сторон на основании размеров
    if(maxX > maxY)
        aspectRatio = maxX/maxY;
    else
        aspectRatio = maxY/maxX;

    ui->statusBar->showMessage("Формирование 3D сцены завершено",-1);
    camera = view->camera();
    //для каждой сцены выставляем свои настройки согласно параметрам отображаемой АР
    camController = new Qt3DExtras::QOrbitCameraController(scene);
    //camera->lens()->setPerspectiveProjection(50.0f, 1.f, 0.1f, 10000.0f);
    camera->lens()->setPerspectiveProjection(45, aspectRatio, 0.1f, maxic*100.);//условие отображения стабильно 3D сцены
    //TODO: отслеживаем положение камеры что бы не подымалась больше генерируемой сцены
    camera->setPosition(QVector3D(0, 0, maxic*10));//300.f
    camera->setTop(maxic);
    //camera->setViewCenter(QVector3D(0, 0, 0));
    camController->setLinearSpeed(maxic);//50.0f
    camController->setLookSpeed(380.0f);
    camController->setCamera(camera);
    update();
}

void MainWindow::zRadialHeight(std::vector<double> X, std::vector<double> Y, std::vector<double> &Z)
{
    double fRmax = 0, coeff;
    for(unsigned int i = 0; i < X.size(); i ++)
    {
        double fR = fabs(sqrt(pow(X[i],2) + pow(Y[i],2)));
        if (fR > fRmax)
            fRmax = fR;
    }

    coeff = -m_Aheigh / fRmax;
    for(int i = 0; i < X.size(); i ++)
    {
        Z[i] = sqrt(pow(X[i],2) + pow(Y[i],2)) * coeff;/// 20); pow(10,
    }
}

void MainWindow::zParabolHeight(std::vector<double> X, std::vector<double> Y, std::vector<double> &Z)
{
    for(unsigned int i = 0; i < X.size(); i++)
        Z.at(i) = (pow(X.at(i),2) + pow(Y.at(i),2))/(4*m_Aheigh);

}

void MainWindow::calculateZCoord()
{
    /// если задано распределение
    if(radial)
        zRadialHeight(x_Pos,y_Pos,z_Pos);
    if(parabol)
        zParabolHeight(x_Pos,y_Pos,z_Pos);
    if (random)
        randomZCoord(z_Pos);
    if (sineM)
       calculateSine(x_Pos,y_Pos,z_Pos);
    if (noDistrib)
        setNoDistrub(z_Pos);
}

void MainWindow::setDefultFlags()
{
    radial      =   false;
    parabol     =   false;
    pyramidal   =   false;
    sineM =         false;
    linear =        false;
    random =        false;
    noDistrib =     false;
    Half =          false;
    Ellipse =       false;
    Ramp =          false;
    Circles =       false;
}

void MainWindow::randomZCoord( std::vector<double> &Z)
{
    for(unsigned int i = 0; i < Z.size(); i++)
        z_Pos.at(i) = (std::abs(rand() % 20 * m_Aheigh));// случайное расположение z-координаты
}

void MainWindow::setNoDistrub(std::vector<double> &Z)
{
    for(unsigned int i = 0; i<Z.size(); i++)
        Z.at(i) = 0.;//расположение z-координаты
}

void MainWindow::calculateSine(std::vector<double> X, std::vector<double> Y, std::vector<double> &Z)
{
    //реализованно
    Norm = m_Aheigh * sin(std::abs(PI * 2 / (sqrt(X.at(0)*X.at(0) + Y.at(0)*Y.at(0)))));
    for (unsigned int i = 0; i < Z.size(); i++)
        Z.at(i) = (std::round(m_Aheigh*sin(std::abs(PI * 2 / (sqrt(X.at(i)*X.at(i) + Y.at(i)*Y.at(i))))) - Norm));//
}

void MainWindow::SaveToFile()
{
    QString m_szFileName = QFileDialog::getSaveFileName(this, "Save",
                                                        tr("output.txt"), tr("Txt Files (*.txt)"));

    if (m_szFileName == "")
        return;

    QFile data(m_szFileName);
    if (data.open(QFile::WriteOnly | QFile::Truncate)) {
        QTextStream out(&data);
        for (int i = 0; i < x_Pos.size(); i++)
        {
            out << x_Pos[i] << "\t" << y_Pos[i] << "\t" << z_Pos[i] << "\n";
        }

        data.close();

        QMessageBox::information(this, "Сохранение координат", "Новые координаты сохранены в файл.");
    }
    else
    {
        QMessageBox::critical(this, "Сохранение координат", "Не удалось сохранить координаты!");
    }
}

void MainWindow::elementRange()
{
    DialogButtonBottom* rangeDlg = new DialogButtonBottom(this);
    if (!rangeDlg->exec())
    {
        QMessageBox::warning(this, "Внимание", "Параметры не выбраны");
        return;
    }
    m_beginRange = rangeDlg->beginRanges;
    m_endingRange = rangeDlg->endRanges;

    //заполняем массив согласно диапазонам
    changePositionAndRotationEl = true;
    //обнуляем счетчик на возврат
    currentStep = -1;
    //обнуляем массив
    rendo_changePositionRotation.clear();
    for (unsigned int j = 0; j < m_beginRange.size(); j++)
        for (unsigned int i = m_beginRange.at(j) - 1; i < m_endingRange.at(j); i++)
        {
            //необходимо добавить проверку на наличие выбранного элемента в массиве
            if (m_changePositionRotation.size() > 0)
            {
                bool mark = false;//флаг существования выбранного элемента в массиве для перемещения
                for (int u = 0; u < m_changePositionRotation.size(); u++)
                    if (m_changePositionRotation.at(u).graphicsItem() == m_itemsListBuff.at(i).graphicsItem())
                        mark = true;
                if (!mark)
                {
                    m_changePositionRotation.emplace_back(m_itemsListBuff.at(i));
                    m_changePositionRotation.back().graphicsItem()->setColor(Qt::darkRed);
                }
            }
            else
            {
                m_changePositionRotation.emplace_back(m_itemsListBuff.at(i));
                m_changePositionRotation.back().graphicsItem()->setColor(Qt::darkRed);
            }
        }
    int y = 0;
}

void MainWindow::deleteExistElement()
{
}

void MainWindow::addNewElement()
{
    //осуществим отрисовку элемента как и уже имеющиеся
    createElementPaint(m_itemsListBuff.back().Id() + 1, rectBuff, rectFBuff, setVisibleNum->isChecked(), dlIndex);
    m_itemsListBuff.emplace_back(List(m_itemsListBuff.back().Id() + 1, paintAr, cursorPositionBuff.x(), cursorPositionBuff.y(), 0, false, 0));
    //помещаем новый элемент на сцену
    buffScene->addItem(m_itemsListBuff.back().graphicsItem());
    m_itemsListBuff.back().graphicsItem()->setPos(cursorPositionBuff);

    //передаем новые координаты в обработчик
    x_Pos.emplace_back(cursorPositionBuff.x() * static_cast<double>(scaleW)
                       - static_cast<double>(deltW * scaleW));
    y_Pos.emplace_back(cursorPositionBuff.y() * static_cast<double>(scaleH)
                       - static_cast<double>(deltH * scaleH));
    z_Pos.emplace_back(0.);
    //
    CanChange = false;
    //заполням таблицу
    ui->TwCoordBrowser->setRowCount(static_cast<int>(m_itemsListBuff.back().Id() + 1));
    ui->TwCoordBrowser->setItem(m_itemsListBuff.back().Id(), COL_X,
                                new QTableWidgetItem(QString("%1").arg(x_Pos.back())));
    ui->TwCoordBrowser->setItem(m_itemsListBuff.back().Id(), COL_Y,
                                new QTableWidgetItem(QString("%1").arg(y_Pos.back())));
    //
    CanChange = true;

}

void MainWindow::mousePressEvent(QMouseEvent *event)
{

    if (event->button() == Qt::MidButton)
    {
        if (!pressed)
        {
            this->setCursor(QCursor(Qt::OpenHandCursor));
            QGraphicsView::DragMode mode;
            mode = QGraphicsView::ScrollHandDrag;
            gViewElements->setDragMode(mode);
            gViewModule->setDragMode(mode);
            pressed = true;
        }
        else
        {
            this->setCursor(QCursor(Qt::ArrowCursor));
            QGraphicsView::DragMode mode;
            mode = QGraphicsView::NoDrag;
            gViewElements->setDragMode(mode);
            gViewModule->setDragMode(mode);
            pressed = false;
        }
    }
}

void MainWindow::markedChooseTab(int index)
{
    tabIndex = index;
    switch (index)
    {
    case MODULES:
        //делаем перенос массивов элементов
        //m_itemsListBuff = m_itemsListM;
        //
        //m_stepBuff = m_stepM;
        //
        buffScene = sceneM;
        //определяем буфер для отслеживания отображения
        //scaleH = scaleH;
        //scaleW = scaleW;
        //deltW = deltW;
        //deltH = deltH;
        //буфер координат
        //        x_Pos = x_Pos;
        //        y_Pos = y_Pos;
        //        z_Pos = z_Pos;

        TableInit();
        //необходимо перезаполнять таблицу
        for (unsigned int i = 0; i < x_Pos.size(); i++)
        {
            ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(x_Pos.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(y_Pos.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(z_Pos.at(i))));
        }
        break;
    case ELEMENTS:
        //        x_Pos = x_Pos;
        //        y_Pos = y_Pos;
        //        z_Pos = z_Pos;

        //if (m_itemsListBuff.size() == m_itemsListM.size())
        //    m_itemsListM = m_itemsListBuff;
        //делаем перенос массивов элементов
        //m_itemsListBuff = m_itemsListE;
        //
        //m_stepBuff = m_stepE;
        //
        buffScene = sceneE;
        //определяем буфер для отслеживания отображения
        //scaleH = coeffH;
        //scaleW = coeffW;
        //deltW = deltWE;
        // deltH = deltHE;
        //буфер координат
        //        x_Pos = tXe;
        //        y_Pos = tYe;
        //        z_Pos = tZe;
        //инициализируекм таблицу
        TableInit();
        //необходимо перезаполнять таблицу
        for (unsigned int i = 0; i < tXe.size(); i++)
        {
            ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(tXe.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(tYe.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(tZe.at(i))));
        }
        break;
    }
}

void MainWindow::setRotateTypeAsElCentr()
{
    rotateElementOnCentr = true;
    rotateElementsOnCentr = false;
    rotateElemsCentr->setChecked(false);
    //сохраняем угол изменения при смене типа поворота
    //    rotateOnCenter = rotateAngle;
    //    rotateAngle = rotateOnAxie;
}

void MainWindow::setRotateTypeAsElsCentr()
{
    rotateElementsOnCentr = true;
    rotateElementOnCentr = false;
    rotateElemCentr->setChecked(false);
    //сохраняем угол изменения при смене типа поворота
    //    rotateOnAxie = rotateAngle;
    //    rotateAngle = rotateOnCenter;
}

void MainWindow::changeMousePositionForElements(QPointF point, int ID)
{
    //сюда приходит значение разницы
    releasePoint = point;// m_itemsListBuff.at(static_cast<unsigned int>(ID - 1)).graphicsItem()->pos();//запоминаем дельту
    CanChange = false;
    if (m_changePositionRotation.size() > 0)
        for (unsigned int i = 0; i < m_changePositionRotation.size(); i++)
        {
            //осуществляем перемещение для всех элементов массива
            m_changePositionRotation.at(i).graphicsItem()->setPos((x_Pos.at(m_changePositionRotation.at(i).Id())) / static_cast<double>(scaleW)
                                                                  + gViewModule->width()/2 + point.x(),
                                                                  (-y_Pos.at(m_changePositionRotation.at(i).Id()))/ static_cast<double>(scaleH)
                                                                  + gViewModule->height()/2 + point.y());

            //записываем новые координаты в таблицу
            ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_X,
                                        new QTableWidgetItem(QString("%1").arg(x_Pos.at(m_changePositionRotation.at(i).Id()) + point.x())));
            ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_Y,
                                        new QTableWidgetItem(QString("%1").arg(y_Pos.at(m_changePositionRotation.at(i).Id()) - point.y())));

            //markedChangedItem(m_changePositionRotation.at(i).Id());
        }
    CanChange = true;
}

MainWindow::MainWindow(QWidget *parent) :
    QMainWindow(parent),
    ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    //
    colorList = new ColorListEditor();
    colorList->setColor("white");

    container->setSizePolicy(QSizePolicy::Expanding,QSizePolicy::Expanding);
    //вручную прописанные элементы
    //QLabel* l_fon = new QLabel("Фоновый Цвет");
    //l_fon->setFont(QFont("Times New Roman",9));
    //ui->gridLayout_3->addWidget(l_fon,6,0);
    //ui->gridLayout_3->addWidget(colorList, 6,1);
    //
    ui->tabWidget->setVisible(false);
    ui->TwCoordBrowser->setVisible(false);
    //
    startLabel();
    //
    ui->progressBar->setVisible(true);
    ui->methodBox->setVisible(true);
    ui->lineEdit_2->setVisible(true);
    ui->l_coeff->setVisible(true);
    ui->checkBox->setVisible(true);
    ui->cBGenAR->setVisible(true);
    ui->lE_Rotation->setVisible(true);
    ui->lRotate->setVisible(true);
    ui->pBAdedExist->setVisible(false);
    Chosemode = true;
    //делает цвет фона по желанию
    ui->lineEdit->setText("файл не выбран");
    ui->progressBar->setValue(0.0);
    ui->lE_Rotation->setText(QString::number(0.0));
    //обработка сетки
    visibleGrid = false;
    ui->cHGrid->setChecked(false);
    keyStep = 1.0;
    zHeight = 300.;
    ui->dsBStep->setValue(keyStep);
    header << trUtf8("X, мм")
           << trUtf8("Y, мм")
           << trUtf8("Z, мм");
    ui->TwCoordBrowser->horizontalHeader()->setVisible(true);
    ui->TwCoordBrowser->setHorizontalHeaderLabels(header);
    ui->TwCoordBrowser->horizontalHeader()->setStretchLastSection(true);
    //описание новых элементов
    infoLable = new QLabel();
    infoLable->setFixedWidth(300);
    gViewModule = new QGraphicsView();
    gViewElements = new QGraphicsView();

    gViewModule->setHorizontalScrollBarPolicy(Qt::ScrollBarAlwaysOff);
    gViewModule->setVerticalScrollBarPolicy(Qt::ScrollBarAlwaysOff);

    Graphics_view_zoom* z = new Graphics_view_zoom(gViewModule);
    z->set_modifiers(Qt::NoModifier);
    Graphics_view_zoom* f = new Graphics_view_zoom(gViewElements);
    f->set_modifiers(Qt::NoModifier);

    gridBack = nullptr;

    pressed = false;
    loadBlock = false;
    dlgIsOpen = false;
    addedElements = false;
    autoCalculate = false;

    ui->tabWidget->clear();//убираем лишние табы
    ui->tabWidget->addTab(gViewModule, "Работа с модулями АР");
    ui->tabWidget->addTab(gViewElements, "Работа с элементами АР");

    ui->gridLayout->addWidget(infoLable, 1, 0, Qt::AlignTop);

    QFont font = QFont("Times New Roman", 14);
    font.setBold(true);
    ui->TwCoordBrowser->setFont(font);
    infoLable->setFont(font);

    //изначально доплнительный режим и 3D отображение сокрыты
    mod3DOn = false;
    CanChange = false;
    m_doubleClick = false;
    firstInit = true;
    ui->cBGenAR->setChecked(true);
    GenerationAR = true;//поумолчанию работает расчет с формированием АР
    //переменные регулируют тип вращения и перерасчет координат
    rotateElementOnCentr = true;
    rotateElementsOnCentr = false;
    //флаг на поворот или перемещение объектов выделенных
    changePositionAndRotationEl = false;

    obj = new objectAR;
    m_Aheigh = ui->lineEdit_2->text().toDouble();


    ui->methodBox->addItem("Пирамидальное распределение", PYRIMIDE);
    ui->methodBox->addItem("Разобщенное распределение", SINE);
    ui->methodBox->addItem("Линейное распределение", LINEAR);
    ui->methodBox->addItem("Случайное распределение", RANDOM);
    ui->methodBox->addItem("Радиальное распределение", RADIAL);
    ui->methodBox->addItem("Параболическое распределение", PARABOL);
    ui->methodBox->addItem("Без распределения", NODISTRIB);
    ui->methodBox->setCurrentIndex(6);

    inFile = false;//говорим что не принимаем данные из файла
    diagIndex = 0;//индекс всегда равен 0 для стандартного случая
    rotateAngle = 0.f;

    first3D = true;//означает первый запуск 3D
    //формируем контейнер
    paintAr = new PaintAperture(0, QRect(0, 0, 0, 0), QRectF(0, 0, 0, 0), false, false,Qt::darkGreen);
    sLight = new Qt3DRender::QSpotLight;
    scene = createScene();
    camController = new Qt3DExtras::QOrbitCameraController(scene);
    camera->lens()->setPerspectiveProjection(50.0f, 160.0f / 9.0f, 0.1f, 10000.0f);
    camera->setPosition(QVector3D(0, 0, 300.0f));
    camera->setViewCenter(QVector3D(0, 0, 0));
    camController->setLinearSpeed(50.0f);
    camController->setLookSpeed(380.0f);
    camController->setCamera(camera);


    rootEntity = new Qt3DCore::QEntity();

    pyramidal = true;
    sineM = false;
    linear = false;
    random = false;
    //функция заполнения помощников
    setToolTipOnUI();
    //Функция настроек
    startSettings();
    //Функция формирования остальных объектов быстрого меню
    menuCall();

    setVisibleNum = new QAction(QIcon(""), "Отображение номеров");
    setVisibleNum->setCheckable(true);
    setVisibleNum->setChecked(false);

    setVisibleNumFastening = new QAction(QIcon(""), "Отображение Креплений");
    setVisibleNumFastening->setCheckable(true);
    setVisibleNumFastening->setChecked(false);

    rotateElemCentr = new QAction(QIcon(""), "Вращение вокруг центра элемента");
    rotateElemCentr->setCheckable(true);
    rotateElemCentr->setChecked(true);

    rotateElemsCentr = new QAction(QIcon(""), "Вращение вокруг центра элементов");
    rotateElemsCentr->setCheckable(true);
    rotateElemsCentr->setChecked(false);

    checkElementsRange = new QAction(QIcon(""), "Выбрать элементы");

    newElement = new QAction(QIcon(""), "Новый элемент");
    deleteElement = new QAction(QIcon(""), "Удалить элемент");

    saveInFile = new QAction(QIcon(""), "Сохранить в файл");

    setCoords = new QAction(QIcon(""), "Подгрузить элементы одного модуля");
    //подгружаем координаты элементов модулей и отрисовываем
    connect(setCoords, SIGNAL(triggered(bool)), this, SLOT(drawModuleElements()));
    connect(ui->tabWidget, SIGNAL(tabBarClicked(int)), this, SLOT(markedChooseTab(int)));
    connect(rotateElemCentr, SIGNAL(triggered(bool)), this, SLOT(setRotateTypeAsElCentr()));
    connect(rotateElemsCentr, SIGNAL(triggered(bool)), this, SLOT(setRotateTypeAsElsCentr()));
    connect(saveInFile, SIGNAL(triggered(bool)), this, SLOT(SaveToFile()));
    connect(checkElementsRange, SIGNAL(triggered(bool)), this, SLOT(elementRange()));
    connect(newElement, SIGNAL(triggered(bool)), this, SLOT(addNewElement()));
    connect(deleteElement, SIGNAL(triggered(bool)), this, SLOT(deleteExistElement()));
    connect(colorList, SIGNAL(currentIndexChanged(int)), this, SLOT(drawBackGroundGrid()));
    count = 0;
    currentStep = -1;
    rotateOnAxie = 0.f;
    rotateOnCenter = 0.f;
}

MainWindow::~MainWindow()
{
    freeMemory();
    x_Pos3D.clear();
    y_Pos3D.clear();
    z_Pos3D.clear();
    //очищаем область модулей
    if(gViewModule->items().size()>0)
    {
        for(int i = 0; i < gViewModule->items().size(); i++)
            delete gViewModule->items().at(i);
        delete gViewModule;
    }
    //очищаем область элементов
    if(gViewElements->items().size()>0)
    {
        for(int i = 0; i < gViewElements->items().size(); i++)
            delete gViewElements->items().at(i);
        delete gViewElements;
    }
    //очищаем 3D
    if(scene->components().size()>0)
    {
        for(int i = 0; i < scene->components().size(); i++)
            scene->removeComponent(scene->components().at(i));
        delete scene;
    }
    delete colorList;
    delete firstLabel;
    delete container;
    delete ui;
}

void MainWindow::keyPressEvent(QKeyEvent *ev)
{
    switch (ev->key())
    {
    case Qt::Key_Escape:
        m_doubleClick = false;
        changePositionAndRotationEl = false;
        m_changePositionRotation.clear();
        rendo_changePositionRotation.clear();
        emit disableCheck();
        break;

    case Qt::Key_W:
        if (m_doubleClick || changePositionAndRotationEl)
        {
            typeKey = UP;
            addValueKey(m_IDitem);
        }
        break;

    case Qt::Key_S:
        if (QGuiApplication::keyboardModifiers() == Qt::ControlModifier)
        {
            QPixmap screenshot;
            if(container->isVisible())
                screenshot = container->grab();
            else
                screenshot = ui->tabWidget->grab();
            QString fileName = QFileDialog::getSaveFileName(this,
                                                            tr("Save Address Book"), "",
                                                            tr("Address Book (*.png);;All Files (*)"));
            screenshot.save(fileName, "png");
            break;
        }
        if (m_doubleClick || changePositionAndRotationEl) {
            typeKey = DOWN;
            addValueKey(m_IDitem);
        }
        break;

    case Qt::Key_A:
        if (m_doubleClick || changePositionAndRotationEl) {
            typeKey = LEFT;
            addValueKey(m_IDitem);
        }
        break;

    case Qt::Key_D:
        if (m_doubleClick || changePositionAndRotationEl) {
            typeKey = RIGHT;
            //m_itemsListBuff.at(static_cast<unsigned int>(m_IDitem - 1)).graphicsItem()->setSelected(true);
            addValueKey(m_IDitem);
        }
        break;

    case Qt::Key_Z:
        if (QGuiApplication::keyboardModifiers() == Qt::ControlModifier)
        {
            Undo();
        }
        break;
    case Qt::Key_Y:
        if (QGuiApplication::keyboardModifiers() == Qt::ControlModifier)
        {
            Rendo();
        }
        break;

    case Qt::Key_L:
        if (m_doubleClick || changePositionAndRotationEl) {
            if(!changePositionAndRotationEl)
                m_itemsListBuff.at(static_cast<unsigned int>(m_IDitem)).graphicsItem()->setSelected(true);
            keyOnRotate(TURNRIGHT, m_IDitem);
            //rotateElementOnCentr = true;
            //rotateElementsOnCentr = false;
        }
        break;
    case Qt::Key_R:
        if (m_doubleClick || changePositionAndRotationEl) {
            if(!changePositionAndRotationEl)
                m_itemsListBuff.at(static_cast<unsigned int>(m_IDitem)).graphicsItem()->setSelected(true);
            keyOnRotate(TURNLEFT, m_IDitem);
            //rotateElementOnCentr = true;
            //rotateElementsOnCentr = false;
        }
        break;
    case Qt::Key_C:
        if (QGuiApplication::keyboardModifiers() == Qt::ControlModifier)
        {
            QClipboard* clipBoard = QApplication::clipboard();
            QPixmap screenshot;
            if(container->isVisible())
                screenshot = container->grab();
            else
                screenshot = ui->tabWidget->grab();
            clipBoard->setPixmap(screenshot);
        }
        break;
    default:
        QWidget::keyPressEvent(ev);
        break;
    }

}

void MainWindow::TableInit()
{
    ui->TwCoordBrowser->clear();
    ui->TwCoordBrowser->setRowCount(0);
    header << trUtf8("X,мм")
           << trUtf8("Y, мм")
           << trUtf8("Z, мм");
    ui->TwCoordBrowser->horizontalHeader()->setVisible(true);
    ui->TwCoordBrowser->setHorizontalHeaderLabels(header);
    ui->TwCoordBrowser->horizontalHeader()->setStretchLastSection(true);

    QFont font = QFont("Times New Roman", 14);
    font.setBold(true);
    ui->TwCoordBrowser->setFont(font);

    CanChange = false;
}

void MainWindow::contextMenuEvent(QContextMenuEvent *event)
{
    Q_UNUSED(event);

    QCursor cur;
    QMenu *subMenu = new QMenu("Диапазон");
    QMenu *fileMenu = new QMenu("Работа с файлами");

    QMenu menu;
    //делаем под-меню
    subMenu->addAction(checkElementsRange);
    fileMenu->addAction(saveInFile);
    fileMenu->addAction(setCoords);

    menu.addAction(setVisibleNum);
    menu.addAction(rotateElemCentr);
    menu.addAction(rotateElemsCentr);
    if (diagIndex == POLYGON)
        menu.addAction(setVisibleNumFastening);
    menu.addMenu(subMenu);
    menu.addMenu(fileMenu);
    if (sceneM->itemAt(static_cast<qreal>(cur.pos().x()), static_cast<qreal>(cur.pos().y()), QTransform()) != nullptr)
    {
        menu.addAction(deleteElement);
    }
    else
    {
        cursorPositionBuff = cur.pos();
        menu.addAction(newElement);
    }
    //если попадет мышка курсора на элемент
    //menu.addAction(setCoords);
    menu.exec(cur.pos());
}

void MainWindow::addValueMouse(int Row)
{

    if (!changePositionAndRotationEl)
    {

        QPointF point = m_itemsListBuff.at(static_cast<unsigned int>(Row - 1)).graphicsItem()->pos();
        //блок расчета смещения блока
        double deltX, deltY;
        float top = ((point.x() - gViewModule->width()/2))*static_cast<double>(scaleW);
        //- static_cast<double>(deltW));
        //в начале точку центрируем потом меняем размерность
        deltX = x_Pos.at(Row - 1) - ((point.x() - gViewModule->width()/2)*static_cast<double>(scaleW));
        // - static_cast<double>(deltW/scaleW));
        deltY = y_Pos.at(Row - 1) - (-(point.y() - gViewModule->height()/2)*static_cast<double>(scaleH));
        //  - static_cast<double>(deltH));

        x_Pos.at(static_cast<unsigned>(Row - 1)) = (point.x() - gViewModule->width()/2)*static_cast<double>(scaleW);// qRound(releasePoint.x()); //
        ///- static_cast<double>(deltW);///static_cast<double>(scaleW));
        y_Pos.at(static_cast<unsigned>(Row - 1)) = (point.y() - gViewModule->height()/2)*static_cast<double>(scaleH);// qRound(releasePoint.y()); //
        ///- static_cast<double>(deltH);/// static_cast<double>(scaleH));
        z_Pos.at(static_cast<unsigned>(Row - 1)) = 0;
        //переворот координат
        y_Pos.at(static_cast<unsigned>(Row - 1)) = - y_Pos.at(static_cast<unsigned>(Row - 1));

        ui->TwCoordBrowser->setItem(Row - 1, COL_X,
                                    new QTableWidgetItem(QString("%1").arg(x_Pos.at(Row - 1))));
        ui->TwCoordBrowser->setItem(Row - 1, COL_Y,
                                    new QTableWidgetItem(QString("%1").arg(y_Pos.at(Row - 1))));
        ui->TwCoordBrowser->setItem(Row - 1, COL_Z,
                                    new QTableWidgetItem(QString("%1").arg(z_Pos.at(Row - 1))));


        m_itemsListBuff.at(Row - 1).setX(x_Pos.at(Row - 1) / static_cast<double>(scaleW)
                                         + static_cast<double>(gViewModule->width()/2 ));// scaleW));//));
        m_itemsListBuff.at(Row - 1).setY(y_Pos.at(Row - 1) / static_cast<double>(scaleH)
                                         + static_cast<double>(gViewModule->height()/2));// scaleH));//));
        //Запоминаем текущий шаг
        //createStepMemory();
        //markedChangedItem(Row - 1);
        if (addedElements)
            realTimechangePosition(Row, -deltX, deltY);
    }
    else
    {
        if (m_changePositionRotation.size() > 0)
            for (unsigned int i = 0; i < m_changePositionRotation.size(); i++)
            {
                //обрабатываем текущую и предыдущую позицию
                QPointF point = m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).graphicsItem()->pos();
                double deltX, deltY;
                deltX = x_Pos.at(m_changePositionRotation.at(i).Id()) - (point.x() - gViewModule->width()/2)*static_cast<double>(scaleW); //((point.x())*static_cast<double>(scaleW)
                // - static_cast<double>(deltW));
                deltY = y_Pos.at(m_changePositionRotation.at(i).Id()) - (-(point.y() - gViewModule->height()/2)*static_cast<double>(scaleH));//(-((point.y())*static_cast<double>(scaleH)
                //   - static_cast<double>(deltH)));

                //необходимо брать значения напрямую из объекта и пересчитывать под координаты
                x_Pos.at(m_changePositionRotation.at(i).Id()) -= (deltX);
                y_Pos.at(m_changePositionRotation.at(i).Id()) -= (deltY);

                //записываем новые координаты в таблицу
                ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_X,
                                            new QTableWidgetItem(QString("%1").arg(x_Pos.at(m_changePositionRotation.at(i).Id()))));
                ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_Y,
                                            new QTableWidgetItem(QString("%1").arg(y_Pos.at(m_changePositionRotation.at(i).Id()))));

                //
                m_changePositionRotation.at(i).setX(m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).X() + deltX);
                m_changePositionRotation.at(i).setY(m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).Y() + deltY);
                //ЗАПОМИНАЕМ НОВЫЕ координаты
                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setX(m_changePositionRotation.at(i).X());
                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setY(m_changePositionRotation.at(i).Y());

                //осуществляем перемещение для всех элементов массива
                //m_changePositionRotation.at(i).graphicsItem()->setPos(m_changePositionRotation.at(i).X(),
                //	m_changePositionRotation.at(i).Y());
                //
                //x_Pos.at(m_changePositionRotation.at(i).Id()) += releasePoint.x();
                //y_Pos.at(m_changePositionRotation.at(i).Id()) += releasePoint.y();
                //записываем новые координаты в таблицу
                //ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_X,
                //	new QTableWidgetItem(QString("%1").arg(x_Pos.at(m_changePositionRotation.at(i).Id()) + releasePoint.x())));
                //ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_Y,
                //	new QTableWidgetItem(QString("%1").arg(y_Pos.at(m_changePositionRotation.at(i).Id()) + releasePoint.y())));
                //m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setX(m_changePositionRotation.at(i).X());
                //m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setY(m_changePositionRotation.at(i).Y());

                //markedChangedItem(m_changePositionRotation.at(i).Id());
                if (addedElements)
                    realTimechangePosition(m_changePositionRotation.at(i).Id() + 1, -deltX, deltY);
            }
    }

    //CanChange = true;
}

void MainWindow::on_pushButton_load_clicked()
{
    //проверка на отображение виджета
    if(!ui->tabWidget->isVisible())
    {
        container->setVisible(false);
        firstLabel->setVisible(false);
        ui->tabWidget->setVisible(true);
        ui->TwCoordBrowser->setVisible(true);
    }
    //выбираем в диалоге нужный файл
    QString m_szFileName = QFileDialog::getOpenFileName(this,
                                                        tr("Файл координат"), "", tr("Txt Files (*.txt)"));

    if (m_szFileName != "")
    {
        m_objects.clear();
        loadBlock = true;
        ui->pBAdedExist->setVisible(true);
        countColor = 0;//сбрасываем счетчик
        freeMemory();
        //sceneM->setSceneRect(-3,-3,1628,876);
        gViewModule->setScene(sceneM);
        gViewModule->fitInView(sceneM->sceneRect(), Qt::KeepAspectRatio);
        QRectF sceneRec = sceneM->sceneRect();

        if(!createElementsDraw(gViewModule, m_szFileName, sceneM, m_itemsListM))
            return;

        //clearGrid();
        gridBack = nullptr;
        typeIndex = diagIndex;
        dlIndex = diagIndex;
        //выделяем память
        SetCordsOnCenter(rectBuff.width(), rectBuff.height(), 0, true, fileXe, fileYe, gViewModule);
        x_Pos = x_Pos3D;
        y_Pos = y_Pos3D;
        z_Pos = z_Pos3D;

        m_itemsListBuff = m_itemsListM;

        addGrid(gViewModule, sceneM);

        tabIndex = MODULES;
        ui->statusBar->showMessage("Рассчет окончен", -1);
        ui->lineEdit->setText(m_szFileName);
        m_objects.emplace_back(x_Pos.size(),widthModule,heightModule,typeIndex,NumSide,dlgColor);
    }
}

void MainWindow::on_pushButton_clicked()
{
    //выставляем все флаги по умлочанию
    setDefultFlags();
    //обрабатываем флаг 3D
    if(ui->checkBox->isChecked())
    {
        on_checkBox_clicked(false);
        ui->checkBox->setChecked(false);
    }
    //проверка на отображение виджета
    if(!ui->tabWidget->isVisible())
    {
        ui->tabWidget->setVisible(true);
        ui->TwCoordBrowser->setVisible(true);
        firstLabel->setVisible(false);
    }
    //

    loadBlock = true;
    ui->pBAdedExist->setVisible(true);
    // снимаем флаг отображения цифр
    firstElement = true;

    if (GenerationAR)
    {
        m_objects.clear();
        scene->clearPropertyTrackings();
        //если генерируем решетку, то переходим к выбранному флагу
        switch (method)
        {
        case PYRIMIDE:
            pyramidal = true;
            break;
        case SINE:
            sineM = true;
            break;
        case LINEAR:
            linear = true;
            break;
        case RANDOM:
            random = true;
            break;
        case NODISTRIB:
            noDistrib = true;
            break;
        case RADIAL:
            radial = true;
            break;
        case PARABOL:
            parabol = true;
            break;
        }

        if (!InitDlgConfig())
            return;

        //производим расчет по полученным параметрам
        dDiagIndex = typeIndex;
        switch (typeIndex) {
        case RECTANGLE:
            Ramp = true;
            if (!CalculatePosition())
                return;
            break;
        case ELLIPSE:
            Ellipse = true;
            if (!CalculatePosition())
                return;
            break;
        case HALF:
            Half = true;
            if (!CalculatePosition())
                return;
            break;
        case CIRCLES:
            Circles = true;
            if (!CalculateCirclePosition())
                return;
            break;

        }
        m_objects.emplace_back(x_Pos.size(),widthModule,heightModule,diagIndex,NumSide,dlgColor);
        CanChange = true;

    }
    else
    {
        //Отслеживаем загрузку из файла
        if (inFile)
        {
            TableInit();
            CalculateZFromFile(method);
            QFile file("coords.txt");

            if (file.open(QIODevice::WriteOnly | QIODevice::Truncate))
            {
                QTextStream writeStream(&file);
                for (unsigned int i = 0; i < x_Pos.size(); i++)
                {
                    writeStream << QString::number(x_Pos.at(i)) << "\t"
                                << QString::number(y_Pos.at(i)) << "\t"
                                << QString::number(z_Pos.at(i)) << "\n";
                    ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));
                    ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(x_Pos.at(i))));
                    ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(y_Pos.at(i))));
                    ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(z_Pos.at(i))));

                    textbrowser += QString::number(i + 1) + "\t" + QString::number(x_Pos.at(i)) + "\t" + QString::number(y_Pos.at(i)) + "\t" + QString::number(z_Pos.at(i)) + "\n";
                }
            }
            file.close();
            CanChange = true;
        }
        else
        {
            QMessageBox::warning(this, "Передупрждение", "Файл не подгружен");
        }
    }
    camController->setCamera(camera);
    if (mod3DOn)
    {
        // если да, то отображаем
        view->setRootEntity(scene);
        view->show();
    }
    //модуль сетки
    gridBack = nullptr;
    addGrid(gViewModule, sceneM);
}

void MainWindow::wheelEvent(QWheelEvent *event)
{

    switch (tabIndex)
    {
    case MODULES:
        clearGrid();
        addGrid(gViewModule, sceneM);
        break;
    case ELEMENTS:
        // 		clearGrid();
        // 		addGrid(gViewElements,sceneE);
        break;
    default:
        break;
    }
}

void MainWindow::on_lineEdit_2_textChanged(const QString &arg1)
{
    m_Aheigh = arg1.toDouble();
}

bool MainWindow::CalculatePosition()
{
    //запрещаем режиму изменять через таблицу положения элементов
    //очищаем массивы и сцену перед заполнением
    freeMemory();

    //инициализируем наше отображение антенной решетки
    //обнуляем бар прогресса
    double progress = 0.;
    ui->progressBar->setValue(static_cast<int>(progress));

    //Принимаемы параметры с диалога
    int elementOfRow = dlRow;                                    //количество элементов в строке
    int elementOfColumn = dlCol;                                    //количество элементов в столбце
    int partSin = dlpartSine;                           //передаем количество элементов состовляющих полукруг синуса
    int stepBetweenCols = static_cast<int>(dlBetwCollLength);      //расстояние между элементами по строкам
    int stepBetweenRows = static_cast<int>(dlBetwRowLength);      //расстояние между элементами по колоннам
    //проверяем четночть строк, это влияет на правильность
    double even = elementOfRow % 2 == 0 ? 0.5 : 1;//задаем параметр шага для окружностей по z
    double set = (90. / fabs(partSin - even)); //тем самым формируем полукруг из элементов
    float startAngle = dlAngle;                              //начальная точка полукруга синуса

    //задаем отсчет
    int Id = 0;
    double BuffValue = dlMultValue;
    float coeff = 0.f;
    double stepOnY = 0.;
    float CommonWidth = 0;//смещение блоков по ширине
    float CommonHeigth = 0;// по высоте
    //Создаем массивы под параметры ширины и высоты расположения элементов на сцене
    std::vector<double> xPositionElement;
    std::vector<double> yPositionElement;

    //Расчет коэффициента домнажения для отрисовки элемента
    if (widthModule > heightModule)
        coeff = static_cast<float>(widthModule / heightModule);//пропорция соотношения сторон одного блока
    else
        coeff = static_cast<float>(heightModule / widthModule);
    double A = m_Aheigh;//амплитуда изменения

    //передаем в графический виджет сцену
    gViewModule->setDragMode(QGraphicsView::NoDrag);
    gViewModule->setRenderHint(QPainter::Antialiasing, true);
    gViewModule->setScene(sceneM);
    gViewModule->scene()->setFocus();

    //задаем ей параметры
    sceneM->setSceneRect(-gViewModule->x(), -gViewModule->y(), gViewModule->width(), gViewModule->height());//
    sceneE->setSceneRect(-gViewModule->x(), -gViewModule->y(), gViewModule->width(), gViewModule->height());//
    //запоминаем ширину и высоту сцены для последующего центрирования генерируемых координат
    float ScWidth = static_cast<float>(sceneM->width());
    float ScHeight = static_cast<float> (sceneM->height());
    //вычисляем значение для масштабирования
    float scale = (ScWidth / ScHeight) * 4;

    int collH = static_cast<int>(elementOfRow * elementOfColumn * stepBetweenRows * ((elementOfColumn - ScHeight) / elementOfColumn));//расстояние между элементами
    int collW = static_cast<int>(elementOfRow * elementOfColumn * stepBetweenCols * ((elementOfRow - ScWidth) / elementOfRow));
    float SquareSc = ((ScWidth * ScHeight) - (collH + collW)) / (elementOfRow * elementOfColumn * scale);//расчет площади одного прямоугольника плюс учет расстояния между элементами

    //рассчитываем размеры рисуемых блоков относитеьно полученных значений
    ArWidth = sqrtf(SquareSc / coeff);//282.67f;
    ArHeight = SquareSc / ArWidth;// 326.4f;
    squareBlock = ArWidth * ArHeight;

    //На основе полученных значений формируем область отрисовки и сам рисуемый объект
    QRect rectP(-static_cast<int>(ArWidth), -static_cast<int>(ArHeight),
                static_cast<int>(ArWidth), static_cast<int>(ArHeight));
    //-static_cast<int>(ArWidth), -static_cast<int>(ArHeight),
    //static_cast<int>(ArWidth),static_cast<int>(ArHeight));
    QRectF rectB = QRectF(-static_cast<int>(ArWidth / 2), -static_cast<int>(ArHeight / 2),
                          static_cast<int>(ArWidth), static_cast<int>(ArHeight));
    //запоминаем текущие размеры для рисования элемента
    rectBuff = rectP;
    rectFBuff = rectB;

    //Считаем объем апертуры что будет расположенна на экране с учетом смещения
    for (int i = 0; i < elementOfRow; i++)
        CommonWidth += static_cast<float>(rectP.width() + stepBetweenCols);
    for (int i = 0; i < elementOfColumn; i++)
        CommonHeigth += static_cast<float>(rectP.height() + stepBetweenRows);

    SquareW = CommonWidth;
    SquareH = CommonHeigth;
    //учитываем края АР
    CommonHeigth -= stepBetweenRows;
    CommonWidth -= stepBetweenCols;

    //Задаем стартовые позиции Точку отрисовки первого блока
    double iStartPointX = static_cast<double>(ScWidth / 2 - (CommonWidth / 2) + static_cast<float>(rectP.width() / 2));
    double firstStartPoint = iStartPointX;
    double iStartPointY = static_cast<double>(ScHeight / 2 - (CommonHeigth / 2) + static_cast<float>(rectP.height() / 2));

    //коэффициенты для пирамидального распределения
    //параметры для перебора при формировании пирамидального распределения
    int d = 3, v = 2, z = 1, c = 0;
    //Тело цикла
    for (unsigned int j = 0; j < static_cast<unsigned int>(elementOfColumn); j++)
    {
        //очищаем массив перед использованием
        xPositionElement.clear();
        //множитель для амплитуды при распределении типа эллипс
        BuffValue = j < static_cast<unsigned int>(elementOfColumn) / 2 ? dlMultValue / (j + 1) : dlMultValue / (elementOfColumn - static_cast<int>(j + 1));
        //присвиваем начальную точку
        yPositionElement.emplace_back(iStartPointY);
        for (int i = 0; i < elementOfRow; i++)
        {
            yPositionElement.back() = iStartPointY;
            xPositionElement.emplace_back(iStartPointX);
            createElementPaint(Id, rectP, rectB, setVisibleNum->isChecked(), dlIndex);

            m_itemsListM.push_back(List(Id, paintAr, static_cast<float>(xPositionElement.back()), static_cast<float>(yPositionElement.back()), 0.f, false, 0.f));// x_Pos, y_Pos, z_Pos));
            //передаем на отрисовку
            sceneM->addItem(m_itemsListM.back().graphicsItem());
            //задаем позицию

            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back());

            //проверка на совпадение блок работы коллизий
            while (true)
            {
                //Напишем код коллизий на основе функции
                //collidingItems, что позволит нам определеять
                //положение элемента не только по ширине но и по высоте
                //необходимо учитывать, что при появлении
                int lastItemNumber = i != 0 ? m_itemsListM.back().Id() - 1 : 0;//номер предыдущего элемента

                if (!sceneM->collidingItems(m_itemsListM.back().graphicsItem()).isEmpty())//проверяем на наличие колизий
                {
                    //Если есть колизи с предыдущим элементом, то продолжаем двигать вправо
                    if (!sceneM->collidingItems(m_itemsListM.at(static_cast<unsigned int>(lastItemNumber)).graphicsItem()).isEmpty())//проверка на коллизию с предыдущим элементом
                    {
                        if (i != 0)
                            xPositionElement.back() += 0.1;

                        if (j != 0 && i == 0)//тем самым первый элемент каждой строки будет друг под другом
                            yPositionElement.back() += 0.1;//опускаем вниз и смещаем вправо при синусе
                    }
                    else
                        yPositionElement.back() += 0.1;//опускаем пока не будет коллизии

                    m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back());
                    //также фиксируем точку старта
                    if (i == 0)
                        iStartPointY = yPositionElement.back();
                }
                else
                {
                    if (Ramp)
                    {
                        //положение элементов регулируется в этих двух условиях переменной шага и соответствующими x и y
                        if (i != 0)
                            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back() + stepBetweenCols, yPositionElement.back());
                        //else
                        //    m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back() - stepBetweenRows);
                        if (j != 0)
                        {
                            if (i != 0)
                                m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back() + stepBetweenCols, yPositionElement.back());
                            //else
                            //    m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back() - stepBetweenRows);
                        }
                        m_itemsListM.back().graphicsItem()->setPos( m_itemsListM.back().graphicsItem()->pos().x(), yPositionElement.back() + stepBetweenRows);
                    }
                    //реализация построения половиной апертуры
                    if (Half)
                    {
                        stepOnY = -A * sin(PI * (static_cast<double>(startAngle) + set * i) / 180.0);
                        //положение элементов регулируется в этих двух условиях переменной шага и соответствующими x и y
                        if (i != 0)
                            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back() + stepBetweenCols, yPositionElement.back() - stepOnY);
                        else
                            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back() + stepBetweenRows - stepOnY);
                    }
                    if (Ellipse)
                    {
                        //для эллипса необходимо учитывать распределение и по столбцам
                        stepOnY = A * sin(PI * (static_cast<double>(startAngle) + set * i) / 180.0) * BuffValue;
                        stepOnY = j < static_cast<unsigned int>(elementOfColumn / 2) ? stepOnY :
                                                                                       j == static_cast<unsigned int>(elementOfColumn / 2) ? -stepOnY : 0;//-stepOnY;
                        //                        stepOnX = A * sin(PI * (static_cast<double>(startAngle) + set * abs(elR - i)) / 180.0); //* BuffValue;
                        //                        stepOnX =  j < (elC/ 2) ? stepOnX : stepOnX;
                        //положение элементов регулируется в этих двух условиях переменной coll и соответствующими x и y
                        if (i != 0)
                            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back() + stepBetweenCols, yPositionElement.back() - stepOnY);
                        else
                            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back() + stepBetweenRows - stepOnY);
                    }
                    //запоминаем последние координаты
                    m_itemsListM.back().setX(static_cast<float>(m_itemsListM.back().graphicsItem()->x()));
                    m_itemsListM.back().setY(static_cast<float>(m_itemsListM.back().graphicsItem()->y()));

                    //возвращаем точку отрисовки
                    iStartPointX = i == (elementOfRow - 1) ? firstStartPoint : static_cast<int>(m_itemsListM.back().X());
                    break;
                }
            }

            Id++;
            x_Pos.emplace_back(m_itemsListM.back().X());
            y_Pos.emplace_back(m_itemsListM.back().Y());
            z_Pos.emplace_back(0.);//расположение z-координаты
            rotationArray.emplace_back(0);
            ///
            if (pyramidal)
            {
                if (elementOfRow == 8 && elementOfColumn == 8)
                {
                    if (i == 0 || j == 0 || j == elementOfColumn - 1 || i == elementOfRow - 1) // формируем подложку
                        z_Pos.back() = 0;//значения z координаты по краям АР
                    else
                    {
                        if ((i > c && j == z && i < elementOfRow - v) ||
                                (i > c && j == elementOfColumn - v && i < elementOfRow - z) ||
                                (j > c && i == z && j < elementOfColumn - v) ||
                                (j > c && i == elementOfRow - v && j < elementOfColumn - v))
                        {
                            z_Pos.back() = (A * sin(PI * (startAngle + set * 5) / 180.0f));// +A * cos(PI * (angle + set * j) / 180.0f)));
                        }
                        if (((i > z) && (j == v) && (i < elementOfRow - v)) ||
                                (i > z && j == elementOfColumn - d && i < elementOfRow - v) ||
                                (j > z && i == v && j < elementOfColumn - d) ||
                                (j > z && i == elementOfRow - d && j < elementOfColumn - d))
                            z_Pos.back() = (A * sin(PI * (startAngle + set * 7) / 180.0f));// //+A * cos(PI * (angle + set * j) / 180.0f)));

                        if (i == elementOfRow / 2 - 1 && j == elementOfColumn / 2 - 1 ||
                                i == elementOfRow / 2 && j == elementOfColumn / 2 - 1 ||
                                i == elementOfRow / 2 - 1 && j == elementOfColumn / 2 ||
                                i == elementOfRow / 2 && j == elementOfColumn / 2)
                            z_Pos.back() = A;
                    }
                }
                else
                {
                    QMessageBox::warning(this, "предупреждение", "Для расчета необходимо количество строк и колонн равных 8");
                    freeMemory();
                    return false;
                }
            }
            if (linear)
            {
                if (elementOfRow == 8 && elementOfColumn == 8)// не универсально строго для такого распредедления
                {
                    if (i == 0 || j == 0 || j == elementOfColumn - 1 || i == elementOfRow - 1) // формируем подложку
                        z_Pos.back() = 0;//пока нуль
                    else
                    {
                        if (i > c && j == z && i < elementOfRow - v ||
                                i > c && j == elementOfColumn - v && i < elementOfRow - z ||
                                j > c && i == z && j < elementOfColumn - v ||
                                j > c && i == elementOfRow - v && j < elementOfColumn - v)
                        {
                            z_Pos.back() = (A*(0 + A / 2));// +A * cos(PI * (angle + set * j) / 180.0f)));
                        }
                        if (i > z && j == v && i < elementOfRow - v ||
                                i > z && j == elementOfColumn - d && i < elementOfRow - v ||
                                j > z && i == v && j < elementOfColumn - d ||
                                j > z && i == elementOfRow - d && j < elementOfColumn - d)
                            z_Pos.back() = (A*(1 + A / 2));// //+A * cos(PI * (angle + set * j) / 180.0f)));

                        if (i == elementOfRow / 2 - 1 && j == elementOfColumn / 2 - 1 ||
                                i == elementOfRow / 2 && j == elementOfColumn / 2 - 1 ||
                                i == elementOfRow / 2 - 1 && j == elementOfColumn / 2 ||
                                i == elementOfRow / 2 && j == elementOfColumn / 2)
                            z_Pos.back() = (A*(2 + A / 2));
                    }
                }
                else
                {
                    QMessageBox::warning(this, "предупреждение", "Для расчета необходимо количество строк и колонн равных 8");
                    freeMemory();
                    return false;
                }
            }
            //обновляем статус
            progress += 100.0 / (elementOfRow*elementOfColumn);
            ui->progressBar->setValue(static_cast<int>(round(progress)));
            ui->statusBar->showMessage("Рассчет координат", -1);
        }
    }

    //блок центровки полученных координат
    SetCordsOnCenter(ArWidth, ArHeight, elementOfRow, true, x_Pos, y_Pos, gViewModule);//
    //расчет z координаты
    calculateZCoord();
    //
    currentStep = -1;
    m_stepM.clear();
    changedItem.clear();

    m_itemsListBuff = m_itemsListM;
    buffScene = sceneM;
    //Запоминаем текущий шаг
    createStepMemory(m_stepM);
    //проверяем заполненность таблицы
    TableInit();
    ui->statusBar->showMessage("Рассчет окончен", -1);
    QFile file("coords.txt");

    if (file.open(QIODevice::WriteOnly | QIODevice::Truncate))
    {
        QTextStream writeStream(&file);

        for (unsigned int i = 0; i < x_Pos.size(); i++)
        {
            writeStream << QString::number(x_Pos.at(i)) << "\t"
                        << QString::number(y_Pos.at(i)) << "\t"
                        << QString::number(z_Pos.at(i)) << "\n";

            ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(x_Pos.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(y_Pos.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(z_Pos.at(i))));
        }
    }
    file.close();

    //определяем рабочее поле
    markedChooseTab(MODULES);
    ui->tabWidget->setCurrentIndex(MODULES);

    return true;
}

void MainWindow::CalculateZFromFile(int index)
{
    switch (index)
    {
    case PYRIMIDE:
        //пока не реализовано для универсального типа антенн
        QMessageBox::warning(this, "Передупрждение", "Метод не реализован");
        return;
        // break;
    case SINE:
        calculateSine(x_Pos,y_Pos, z_Pos);
        break;
    case LINEAR:
        //не реализованно для универсальной платформы
        QMessageBox::warning(this, "Передупрждение", "Метод не реализован");
        return;
        //break;
    case RANDOM:
        randomZCoord(z_Pos);
        break;
    case NODISTRIB:
        setNoDistrub(z_Pos);
        break;
    case RADIAL:
        zRadialHeight(x_Pos,y_Pos,z_Pos);
        break;
    case PARABOL:
        zParabolHeight(x_Pos,y_Pos,z_Pos);
        break;
    }

}

void MainWindow::SetCordsOnCenter(float widthCalcElement, float heightCalcElement, int numofRow, bool module, std::vector<double> &X, std::vector<double> &Y,  QGraphicsView* gViewBuff)
{

    //MinMaxCoordsCalc(X, Y);

    if (module)
    {
        //центрирование координат, для оптимизации
        scaleW = static_cast<float>(widthModule) / widthCalcElement;
        scaleH = static_cast<float>(heightModule) / heightCalcElement;

        for (unsigned int i = 0; i < X.size(); i++)
        {
            X.at(i) = X.at(i)*static_cast<double>(scaleW);//переводим координаты к реальному виду
            Y.at(i) = Y.at(i)*static_cast<double>(scaleH);
        }
        ///блок центрирования координат
        ///
        float CenterW = 0.f;
        float CenterH = 0.f;

        switch (dlIndex)
        {
        case RECTANGLE:
            MinMaxCoordsCalc(X, Y);
            CenterW = static_cast<float>(abs(maxX - minX));//
            CenterH = static_cast<float>(abs(maxY - minY));//

            deltW = std::abs(CenterW / 2);//разница для всех координат
            deltH = std::abs(CenterH / 2);//

            break;
        case CIRCLE:
            //
            MinMaxCoordsCalc(X, Y);
            CenterW = static_cast<float>(abs(maxX - minX));//
            CenterH = static_cast<float>(abs(maxY - minY));//

            deltW = std::abs(CenterW / 2);//разница для всех координат
            deltH = std::abs(CenterH / 2);//
            break;
        case POLYGON:
            //
            MinMaxCoordsCalc(X, Y);
            CenterW = static_cast<float>(abs(maxX - minX));//
            CenterH = static_cast<float>(abs(maxY - minY));//

            deltW = std::abs(CenterW / 2);//разница для всех координат
            deltH = std::abs(CenterH / 2);//
            break;
        }


        //блок возврата нормального значения элемента


        for (unsigned int i = 0; i < X.size(); i++)
        {
            X.at(i) = X.at(i) - maxX + static_cast<double>(deltW);//центрирование сетки координат
            Y.at(i) = Y.at(i) - maxY + static_cast<double>(deltH);
        }
        //производим смену знака
        for (unsigned int i = 0; i < X.size(); i++)
        {
            X.at(i) = X.at(i);// -static_cast<double>(deltW);//центрирование сетки координат
            Y.at(i) = -Y.at(i);// -static_cast<double>(deltH);
        }
    }
    else
    {
        //центрирование координат, для оптимизации
        scaleWE = static_cast<float>(widthModule) / widthCalcElement;
        scaleHE = static_cast<float>(heightModule) / heightCalcElement;

        for (unsigned int i = 0; i < X.size(); i++)
        {
            X.at(i) = X.at(i)*static_cast<double>(scaleWE);//переводим координаты к реальному виду
            Y.at(i) = Y.at(i)*static_cast<double>(scaleHE);
        }
        ///блок центрирования координат
        ///
        float CenterW = 0.f;
        float CenterH = 0.f;

        switch (diagIndex)
        {
        case RECTANGLE:
            MinMaxCoordsCalc(X, Y);
            CenterW = static_cast<float>(abs(maxX - minX));//
            CenterH = static_cast<float>(abs(maxY - minY));//

            deltWE = std::abs(CenterW / 2)*gViewBuff->width() / CenterW;//разница для всех координат
            deltHE = std::abs(CenterH / 2)*gViewBuff->height() / CenterH;//

            break;
        case CIRCLE:
            //
            MinMaxCoordsCalc(X, Y);
            CenterW = static_cast<float>(abs(maxX - minX));//
            CenterH = static_cast<float>(abs(maxY - minY));//

            deltWE = std::abs(CenterW / 2)*gViewBuff->width() / CenterW;//разница для всех координат
            deltHE = std::abs(CenterH / 2)*gViewBuff->height() / CenterH;//
            //необходим пересчет дельты
        case POLYGON:
            //
            MinMaxCoordsCalc(X, Y);
            CenterW = static_cast<float>(abs(maxX - minX));//
            CenterH = static_cast<float>(abs(maxY - minY));//

            deltWE = std::abs(CenterW / 2)*gViewBuff->width() / CenterW;//разница для всех координат
            deltHE = std::abs(CenterH / 2)*gViewBuff->height() / CenterH;//
            //необходим пересчет дельты

            break;
        }


        //блок возврата нормального значения элемента

        //
        for (unsigned int i = 0; i < X.size(); i++)
        {
            X.at(i) = X.at(i) - maxX + static_cast<double>(deltWE);//центрирование сетки координат
            Y.at(i) = Y.at(i) - maxY + static_cast<double>(deltHE);
        }
        //считаем дельту относительно экрана
        //производим смену знака
        for (unsigned int i = 0; i < X.size(); i++)
        {
            X.at(i) = X.at(i);// -static_cast<double>(deltW);//центрирование сетки координат
            Y.at(i) = -Y.at(i);// -static_cast<double>(deltH);
        }
    }
    MinMaxCoordsCalc(X, Y);
    //Считаем габариты антенны
    infoLable->setText("Размер антенны по X:" + QString::number(maxX - minX) + "\r\n" + "Размер антенны по Y:" + QString::number(maxY - minY) + "\r\n" + "Внимание:\r\n без учета радиуса элемента");

}

void MainWindow::creatTable(double X, double Y, double Z, int ID)
{
    CanChange = false;
    ui->TwCoordBrowser->setItem(ID, COL_X,
                                new QTableWidgetItem(QString("%1").arg(X*static_cast<double>(scaleW)
                                                                       - static_cast<double>(deltW))));
    ui->TwCoordBrowser->setItem(ID, COL_Y,
                                new QTableWidgetItem(QString("%1").arg(Y*static_cast<double>(scaleH)
                                                                       - static_cast<double>(deltH))));
    CanChange = true;
}

void MainWindow::MinMaxCoordsCalc(std::vector<double> x_P, std::vector<double> y_P)
{
    auto minmaxX = std::minmax_element(x_P.begin(), x_P.end());
    auto minmaxY = std::minmax_element(y_P.begin(), y_P.end());
    //осуществляем пересчет координат под виджет

    minX = *minmaxX.first;
    maxX = *minmaxX.second;
    minY = *minmaxY.first;
    maxY = *minmaxY.second;
}

bool MainWindow::InitDlgConfig()
{
    dlGenConf = new GenerateConf();//инициализируем генератор параметров
    dlGenConf->temeSide(areYouOnDarkSide);
    if (!firstInit)
        dlGenConf->initial(intParametries, floatParametries, doubleParametries, boolParametries);
    else
        firstInit = false;
    if (!dlGenConf->exec())
    {
        QMessageBox::warning(this, "Внимание", "Параметры не выбраны");
        firstInit = true;
        return false;
    }
    //принимаем параметры
    dlRow = dlGenConf->el_row;
    dlCol = dlGenConf->el_coll;
    dlAngle = dlGenConf->m_angle;
    dlIndex = dlGenConf->m_index;
    dlpartSine = dlGenConf->m_partSine;
    dlBetwRowLength = dlGenConf->m_BetwRowLength;
    dlBetwCollLength = dlGenConf->m_BetwCollLength;
    dlMultValue = dlGenConf->m_MultValue;
    dlHightPAA = dlGenConf->highthPAA;
    dlWidthPAA = dlGenConf->widthPAA;
    diagIndex = dlIndex;
    typeIndex = dlGenConf->m_Type;
    dlNumOfElCircle = dlGenConf->m_numOfElCircle;
    dlRotate = dlGenConf->m_Rotate;
    dlCalcEllipse = dlGenConf->calcEllipse;
    //запоминаем в массивы параметров

    intParametries.clear();//предварительно очищаем память
    intParametries.emplace_back(dlRow);
    intParametries.emplace_back(dlCol);
    intParametries.emplace_back(dlIndex);
    intParametries.emplace_back(dlpartSine);
    intParametries.emplace_back(diagIndex);
    intParametries.emplace_back(typeIndex);
    intParametries.emplace_back(dlNumOfElCircle);
    //intParametries.emplace_back(NumSide);

    floatParametries.clear();//предварительно очищаем память
    floatParametries.emplace_back(dlAngle);

    doubleParametries.clear();//предварительно очищаем память
    doubleParametries.emplace_back(dlBetwRowLength);
    doubleParametries.emplace_back(dlBetwCollLength);
    doubleParametries.emplace_back(dlMultValue);
    // 	doubleParametries.emplace_back(dlHightPAA);
    // 	doubleParametries.emplace_back(dlWidthPAA);

    boolParametries.clear();//предварительно очищаем память
    boolParametries.emplace_back(dlRotate);
    boolParametries.emplace_back(dlCalcEllipse);


    switch (dlIndex) {
    case RAMP:
        widthModule = dlGenConf->m_Width;
        heightModule = dlGenConf->m_Height;
        doubleParametries.emplace_back(widthModule);
        doubleParametries.emplace_back(heightModule);
        //учитываем расчет эллипса для разного вида отображения
        if (typeIndex == CIRCLES) {
            doubleParametries.emplace_back(dlHightPAA);
            doubleParametries.emplace_back(dlWidthPAA);
        }
        break;
    case POLYGON:
        heightModule = widthModule = 2 * dlGenConf->m_SideWidth;
        widthSide = dlGenConf->m_SideWidth;
        NumSide = dlGenConf->m_sBNumSide;
        doubleParametries.emplace_back(widthSide);
        doubleParametries.emplace_back(widthSide);//дублируем для удобства последующего запоминания
        if (typeIndex == CIRCLES) {
            doubleParametries.emplace_back(dlHightPAA);
            doubleParametries.emplace_back(dlWidthPAA);
        }
        intParametries.emplace_back(NumSide);
        break;
    case CIRCLE:
        radiusModule = dlGenConf->m_Radius;
        heightModule = widthModule = 2.*dlGenConf->m_Radius;//учитываем что для отображения необходима область в которой будет расположен круг
        doubleParametries.emplace_back(radiusModule);
        doubleParametries.emplace_back(heightModule);
        if (typeIndex == CIRCLES) {
            doubleParametries.emplace_back(dlHightPAA);
            doubleParametries.emplace_back(dlWidthPAA);
        }
        break;
    }
    //
    dlgColor = dlGenConf->elementColor();
    return true;
}

void MainWindow::drawInGraphicScene(std::vector<double> X, std::vector<double> Y,
                                    std::vector<double> Z, QGraphicsView* view,
                                    QGraphicsScene* sceneV, std::vector<List> &itemsGraph)
{
    Q_UNUSED(Z)
    //Не ведем пересчет координат под размеры окна
    //рассчитываем размеры рисуемых блоков относитеьно полученных значений
    float ArWidth = static_cast<float>(widthModule*coeffW);// *scale;//sqrtf(SquareSc/ coeff);
    float ArHeight = static_cast<float>(heightModule*coeffH);// *scale;//SquareSc/ArWidth;

    //зададим выбор параметров блока
    QRect rectA(-static_cast<int>(ArWidth), -static_cast<int>(ArHeight),
                static_cast<int>(ArWidth), static_cast<int>(ArHeight));
    QRectF rectB = QRectF(-static_cast<int>(ArWidth / 2), -static_cast<int>(ArHeight / 2),
                          static_cast<int>(ArWidth), static_cast<int>(ArHeight));
    //запоминаем текущие размеры для рисования элемента
    rectBuff = rectA;
    rectFBuff = rectB;
    int size = itemsGraph.size();
    int j = 0;
    ui->statusBar->showMessage("Отрисовка координат", -1);
    //цикл отрисовки
    for (unsigned int i = size; i < X.size() + size; i++)
    {
        createElementPaint(i, rectA, rectB, setVisibleNum->isChecked(), diagIndex);

        //учитываем количество сторон многоугольника
        itemsGraph.push_back(List(static_cast<int>(i), paintAr, static_cast<float>(X.at(j)), static_cast<float>(Y.at(j)), 0.f, false, 0.f));// , fileXe, fileYe, fileZe));
        sceneV->addItem(itemsGraph.back().graphicsItem());
        itemsGraph.back().graphicsItem()->setPos(X.at(j), Y.at(j));
        j++;
    }
    //контролируем возврат перемещения элементов
    m_stepE.clear();
    currentStep = -1;
    changedItem.clear();
    //m_itemsListBuff = itemsGraph;
    createStepMemory(m_stepE);

    //scene->clearPropertyTrackings();
    //if (scene != nullptr)
    //	delete scene;
    //scene = createScene(x_Pos3D, y_Pos3D, z_Pos3D);
    //camController = new Qt3DExtras::QOrbitCameraController(scene);
    ////    camera->lens()->setPerspectiveProjection(1050.0f, 160.0f/9.0f, 0.1f, 10000.0f);
    //camera->setPosition(QVector3D(0, 0, static_cast<float>(std::abs(x_Pos3D.at(0) - x_Pos3D.back()))));
    ////    camera->setViewCenter(QVector3D(0, 0, 0));
    //camController->setLinearSpeed(static_cast<float>(std::abs(x_Pos3D.at(0) - x_Pos3D.back())) / 2.f);
    ////    camController->setLookSpeed( 180.0f );
    //camController->setCamera(camera);
    //if (mod3DOn)
    //{
    //	view->setRootEntity(scene);
    //	view->show();
    //}
}

void MainWindow::LoadCoordsGview(QString szFileName)
{
    double progress = 0.0;

    ui->progressBar->setValue(static_cast<int>(round(progress)));

    QFile file(szFileName);
    if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
        return;

    double m_iN = 0;
    TableInit();

    fileXe.clear();
    fileYe.clear();
    fileZe.clear();
    x_Pos3D.clear();
    y_Pos3D.clear();
    z_Pos3D.clear();

    //подсчитываем количество элементов в файле координат
    QTextStream in(&file);
    while (!in.atEnd()) {
        QString line = in.readLine();
        m_iN++;
    }

    file.close();

    if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
        return;
    ui->statusBar->showMessage("Загрузка координат", -1);
    float X, Y, Z;
    for (unsigned int i = 0; i < m_iN; i++)
    {
        in >> X >> Y >> Z;
        fileXe.emplace_back(static_cast<double>(X));
        fileYe.emplace_back(static_cast<double>(Y));
        fileZe.emplace_back(static_cast<double>(Z));
        x_Pos3D.emplace_back(static_cast<double>(X));
        y_Pos3D.emplace_back(static_cast<double>(Y));
        z_Pos3D.emplace_back(static_cast<double>(Z));
        rotationArray.emplace_back(0);
        //заполняем браузер координат
        ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));

        //ui->TwCoordBrowser->setItem(static_cast<int>(i),0,new QTableWidgetItem(QString::number(i+1)));
        ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(fileXe.at(i))));//так как отображение перевернуто
        ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(fileYe.at(i))));//то загружаем обратные координаты
        ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(fileZe.at(i))));

        progress += 100.0 / (m_iN);

        ui->progressBar->setValue(static_cast<int>(round(progress)));
    }

    //ui->CoordBrowser->setText(textbrowser);

    file.close();

    MinMaxCoordsCalc(fileXe, fileYe);
    float CenterW = static_cast<float>(abs(maxX) - abs(minX));//
    float CenterH = static_cast<float>(abs(maxY) - abs(minY));//

    for (unsigned int i = 0; i < xLast.size(); i++)
    {
        fileXe.at(i) = fileXe.at(i) - std::abs(CenterW / 2);//центрирование сетки координат
        fileYe.at(i) = fileYe.at(i) - std::abs(CenterH / 2);
    }

    CanChange = true;
    ui->statusBar->showMessage("Загрузка завершина", 10);

    //проверяем подгружаемые координаты
    //если подгрузка блока в место элемента то
    //делаем расчет области отображения относительно
    //модулей

    //    if(!loadBlock)
    //    {
    MinMaxCoordsCalc(fileXe, fileYe);
    ////Находим стороны знаимаемые площадью АР ширину и высоту
    //Находим стороны знаимаемые площадью АР ширину и высоту
    SquareW = std::abs(maxX - minX);//крайнее левое положение координат по X
    SquareH = std::abs(maxY - minY);//крайнее левое положение координат по Y
    //    }
    //необходимо расчитать размер одного элемента
    float scale = (SquareW / SquareH) * 4;//Возьмем универсальное уменьшение
    double coeff;
    coeffH = 1.0;
    coeffW = 1.0;

    //центрируем координаты на области отображения

    double gVwidth = gViewModule->width() / 2;
    double gVheight = gViewModule->height() / 2;

    coeff = (gViewModule->width()*gViewModule->height()) / (SquareH*SquareW);
    scaleHE = scaleWE = static_cast<float>(coeff);
    //Если площадь экрана меньше площади решетки то необходимо пересчитать координаты элементов для экрана
    bool standartCoeff = false;
    for (unsigned int i = 0; i < fileXe.size(); i++)
    {
        fileXe.at(i) = fileXe.at(i);//уходим от центрирования координат для отображения объектов в виджете/difference
        fileYe.at(i) = -fileYe.at(i);// /difference
    }
    if (gViewModule->width() < SquareW)
        scaleWE = coeffW = gViewModule->width() / SquareW;
    if (gViewModule->height() < SquareH)
        scaleHE = coeffH = gViewModule->height() / SquareH;
    //если один из коэффициентов не изменен то присваиваем новое значение каждому
    //учитываем условие окружности
    if (widthModule != heightModule)
    {
        if (coeffH == 1.0 || coeffW == 1.0)
        {
            if (coeffW != 1.0)
                coeff = coeffW;
            if (coeffH != 1.0)
                coeff = coeffH;
        }
        else
            standartCoeff = true;
    }
    else
    {
        if (coeffH < coeffW)
            coeffW = coeffH;
        else
            coeffH = coeffW;
        standartCoeff = true;
    }
    for (unsigned int i = 0; i < fileXe.size(); i++)
    {
        fileXe.at(i) = (fileXe.at(i)*((!standartCoeff) ? coeff : coeffW) + static_cast<double>(gVwidth));////уходим от центрирования координат для отображения объектов в виджете/difference
        fileYe.at(i) = (fileYe.at(i)*((!standartCoeff) ? coeff : coeffH) + static_cast<double>(gVheight));// /difference
    }
    //соотносим координаты с размером окна

    return;
}

void MainWindow::on_methodBox_currentIndexChanged(int index)
{
    method = index;
}

void MainWindow::on_checkBox_clicked(bool checked)
{
    if (checked)
    {
        if(firstLabel->isVisible())
            firstLabel->setVisible(false);
        //формируем параемтры
        create3DSceneParameters();
        //проверяем условие
        if (first3D)
        {
            ui->verticalLayout_2->addWidget(container, 0);
            first3D = false;
        }
        container->setVisible(true);
    }
    else
    {
        if(scene->components().size()>0)
        {
            for(int i = 0; i < scene->components().size(); i++)
                scene->removeComponent(scene->components().at(i));
            delete scene;
            delete camera;
            delete camController;
        }
        mod3DOn = false;
        container->setVisible(false);
        ui->tabWidget->setVisible(true);
    }
    update();
}

void MainWindow::on_cBGenAR_stateChanged(int arg1)
{
    GenerationAR = arg1;
}

void MainWindow::reCalcPositionElements(QTextStream &streamFile, int j)
{
    buffX = xLast = x_PosEl;
    buffY = yLast = y_PosEl;
    //сначало необходимо посчитать новые положения
    //элементов относительно центра, а потом относительно модулей
    m_itemsListBuff.at(static_cast<unsigned int>(j)).graphicsItem()->setSelected(true);
    RotationElements(buffX, buffY, xLast, yLast, -m_itemsListBuff.at(j).getRotateAngle());//необходимо передавать отрицательное
    //добавляем функцию перерасчета для каждого модуля относительно центров модулей
    calcModulsCoords(x_Pos.at(j), y_Pos.at(j), j);
    //запоминаем новые положения координат в спивок
    m_itemsListBuff.at(j).setElementsCoordX(xLast);
    m_itemsListBuff.at(j).setElementsCoordY(yLast);
    //вращаем
    //m_itemsListBuff.at(j).graphicsItem()->setRotation(static_cast<qreal>(m_itemsListBuff.at(j).getRotateAngle()));
    m_itemsListBuff.at(j).setRotateAngle(m_itemsListBuff.at(j).getRotateAngle());
    //запоминаем угол для пересчета в разнице
    rotationArray.at(j) = -m_itemsListBuff.at(j).getRotateAngle();
    for (unsigned int i = 0; i < m_itemsListBuff.at(j).ElementsCoordX().size(); i++)
    {
        //записываем отцентрованные координаты
        // xLast.at(i) = m_itemsList.at(j).ElementsCoordX().at(i);// - static_cast<double>(deltW);
        // yLast.at(i) = m_itemsList.at(j).ElementsCoordY().at(i);// - static_cast<double>(deltH);
        ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));
        ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(xLast.at(i))));
        ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(yLast.at(i))));
        ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(z_PosEl.at(i))));
        //запоминаем координаты в буферные перменные
        tXe.emplace_back(xLast.at(i));
        tYe.emplace_back(yLast.at(i));
        tZe.emplace_back(z_PosEl.at(i));

        streamFile << QString::number(xLast.at(i)) << "\t"
                   << QString::number(yLast.at(i)) << "\t"
                   << QString::number(z_PosEl.at(i)) << "\n";
    }
}

void MainWindow::realTimechangePosition(int numberModule, int stepX, int stepY)
{
    int j = numberModule - 1;
    int range = blockElementsNumber * numberModule;
    buffX = xLast = x_PosEl;
    buffY = yLast = y_PosEl;
    //
    for (unsigned int i = range - blockElementsNumber; i < range; i++)
    {
        // переведем на использование координат из файла
        //xLast.emplace_back();//m_itemsListE.at(i).graphicsItem()->pos().x()
        //yLast.emplace_back();//m_itemsListE.at(i).graphicsItem()->pos().y()
        tXe.at(i) += stepX;
        tYe.at(i) += stepY;
    }
    //определяем предел изменения
    //центрируем положение элементов
    MinMaxCoordsCalc(xLast, yLast);
    float CenterW = static_cast<float>(abs(maxX - minX));//
    float CenterH = static_cast<float>(abs(maxY - minY));//

    for (unsigned int i = 0; i < xLast.size(); i++)
    {
        xLast.at(i) = xLast.at(i) - maxX + std::abs(CenterW / 2);//центрирование сетки координат
        yLast.at(i) = yLast.at(i) - maxY + std::abs(CenterH / 2);
    }
    //

    double gVwidth = gViewModule->width() / 2;
    double gVheight = gViewModule->height() / 2;

    RotationElements(buffX, buffY, xLast, yLast, -m_itemsListBuff.at(j).graphicsItem()->rotation());//необходимо передавать отрицательное

    calcModulsCoords(x_Pos.at(j),// * static_cast<double>(coeffW)
                     //+ static_cast<double>(gVwidth),
                     y_Pos.at(j)//* static_cast<double>(coeffH)
                     //+ static_cast<double>(gVheight)
                     , j);
    //    calcModulsCoords(stepX,
    //                     -stepY, j);
    //блок перерасчета области
    int count = 0;
    for (unsigned int i = range - blockElementsNumber; i < range; i++)
    {
        m_itemsListE.at(i).graphicsItem()->setPos(xLast.at(count)* static_cast<double>(coeffW) + static_cast<double>(gVwidth),
                                                  -yLast.at(count)* static_cast<double>(coeffH) + static_cast<double>(gVheight));
        count++;
    }
    buffX.clear();
    buffY.clear();
    xLast.clear();
    yLast.clear();
}

void MainWindow::realTimeRotation(int ID, float angleStep)
{
    int j = ID - 1;
    int range = blockElementsNumber * ID;
    //std::vector<double> x_ch, y_ch;

    buffX = xLast = x_PosEl;
    buffY = yLast = y_PosEl;

    //    for (unsigned int i = range - blockElementsNumber; i < range; i++)
    //    {
    //        x_ch.emplace_back(m_itemsListE.at(i).graphicsItem()->pos().x());
    //        y_ch.emplace_back(m_itemsListE.at(i).graphicsItem()->pos().y());
    //    }
    //центрируем положение элементов
    MinMaxCoordsCalc(xLast, yLast);
    float CenterW = static_cast<float>(abs(maxX - minX));//
    float CenterH = static_cast<float>(abs(maxY - minY));//

    for (unsigned int i = 0; i < xLast.size(); i++)
    {
        xLast.at(i) = xLast.at(i) - maxX + std::abs(CenterW / 2);//центрирование сетки координат
        yLast.at(i) = yLast.at(i) - maxY + std::abs(CenterH / 2);
    }
    //
    RotationElements(buffX, buffY, xLast, yLast, angleStep);//необходимо передавать отрицательное
    //    for (unsigned int i = 0; i < xLast.size(); i++)
    //    {
    //        xLast.at(i) = xLast.at(i)*coeffW;//scaleW;
    //        yLast.at(i) = yLast.at(i)*coeffH;///scaleH;
    //    }

    double gVwidth = gViewModule->width() / 2;
    double gVheight = gViewModule->height() / 2;

    //определяем позицию на экране
    //QPointF point = m_itemsListBuff.at(ID - 1).graphicsItem()->pos();
    //фозвращаем повернутый объект
    calcModulsCoords(x_Pos.at(j),
                     -y_Pos.at(j) , j);

    //блок перерасчета области
    int count = 0;
    for (unsigned int i = range - blockElementsNumber; i < range; i++)
    {
        m_itemsListE.at(i).graphicsItem()->setPos(xLast.at(count) * static_cast<double>(coeffW)
                                                  + static_cast<double>(gVwidth),
                                                  yLast.at(count) * static_cast<double>(coeffH)
                                                  + static_cast<double>(gVheight));
        count++;
    }

    //записываем новые координаты
    count = 0;
    for (int y = range - blockElementsNumber; y < range; y++)
    {
        tXe.at(y) = xLast.at(count);//*static_cast<double>(coeffW)
        //- static_cast<double>(deltWE);
        tYe.at(y) = yLast.at(count);//*static_cast<double>(coeffH)
        // - static_cast<double>(deltHE);

        count++;
    }

    //    x_ch.clear();
    //    y_ch.clear();
    xLast.clear();
    yLast.clear();
    buffX.clear();
    buffY.clear();
}

void MainWindow::setToolTipOnUI()
{
    QFont font = QFont("Segoe UI",9,true);
    QString text = "Шаг перемещения элементов \n "
                   " при использовании клавиш:\n "
                   "       W A S D            \n "
                   "Шаг  вращения  элементов  \n "
                   " при использовании клавиш:\n "
                   "         R L                ";
    ui->l_step->setFont(font);
    ui->l_step->setToolTip(text);
    text = "Угол текущего отклонения\n"
           "элементов от нормали";
    ui->lRotate->setFont(font);
    ui->lRotate->setToolTip(text);
    text = "коэффициент влияет на высоту\n"
           "генерируемых элементов\n"
           "взависимости от выбранного\n"
           "типа распределения";
    ui->l_coeff->setFont(font);
    ui->l_coeff->setToolTip(text);
    text = "Флаг учета применения распределения\n"
           "к генерируемым элементам";
    ui->cBGenAR->setFont(font);
    ui->cBGenAR->setToolTip(text);

    ui->label_2->setFont(font);
    ui->pushButton->setFont(font);
    ui->cHGrid->setFont(font);
    text = "Режим отображения элементов в объеме";
    ui->checkBox->setFont(font);
    ui->checkBox->setToolTip(text);
    text = "Цвет фона в 2D режиме";
    colorList->setFont(font);
    colorList->setToolTip(text);

}

void MainWindow::startSettings()
{
    connect(ui->settingsWindow, SIGNAL(triggered(bool)), this, SLOT(openSettingsWindow()));
    //Используем системные настройки
    QSettings geomMasterSettings(ORGANIZATION_NAME, APPLICATION_NAME);

    if(geomMasterSettings.value(DARKTEME).toBool())
    {
        areYouOnDarkSide = geomMasterSettings.value(DARKTEME).toBool();
        QFile file(":/styles/darkStyle.qss");
        if(!file.open(QIODevice::ReadOnly))
            return;
        QString stylesheet = QString::fromUtf8(file.readAll());
        this->setStyleSheet(stylesheet);
        colorList->setColor(QColor("transparant"));
    }
    else
    {
        QFile file(":/styles/lightStyle.qss");
        if(!file.open(QIODevice::ReadOnly))
            return;
        QString stylesheet = QString::fromUtf8(file.readAll());
        this->setStyleSheet(stylesheet);
        geomMasterSettings.setValue(DARKTEME,false);
        areYouOnDarkSide =  geomMasterSettings.value(DARKTEME).toBool();
        colorList->setColor(QColor("white"));
    }

    if(geomMasterSettings.value(OPEN3DDLG).toBool())
        dlgIsOpen = geomMasterSettings.value(OPEN3DDLG).toBool();
    else
    {
        dlgIsOpen = geomMasterSettings.value(OPEN3DDLG).toBool();
        //geomMasterSettings.setValue(OPEN3DDLG,false);
    }
    if(geomMasterSettings.value(AUTOCALC).toBool())
        autoCalculate = geomMasterSettings.value(AUTOCALC).toBool();
    else{
        autoCalculate = geomMasterSettings.value(AUTOCALC).toBool();
        //geomMasterSettings.setValue(OPEN3DDLG,autoCalculate);
    }

}

void MainWindow::menuCall()
{
    connect(ui->Help,SIGNAL(triggered(bool)), this, SLOT(openHelpWindow()));
    connect(ui->about,SIGNAL(triggered(bool)), this,SLOT(openAboutWindow()));
    connect(ui->actionPrintToFile, SIGNAL(triggered(bool)),this, SLOT(SaveToFile()));
    connect(ui->open, SIGNAL(triggered(bool)),this,SLOT(on_pushButton_load_clicked()));
}

void MainWindow::addGrid(QGraphicsView* view, QGraphicsScene* grapScene)
{
    gridBack = new BackgroundGrid();
    gridBack->isVisible(visibleGrid);
    QPolygonF poly = view->mapToScene(0, 0, view->width(), view->height());
    //QPolygonF polywin = view->mapToScene(0, 0, view->viewport()->width(), view->viewport()->height());
    gridBack->setRect(QRectF(poly.at(0).x(), poly.at(0).y(), poly.at(2).x(), poly.at(2).y()));//
    gridBack->setBoundingRect(QRectF(-poly.at(2).x() / 2, -poly.at(2).y() / 2, poly.at(2).x(), poly.at(2).y()));
    grapScene->addItem(gridBack);
    gridBack->setBoundingRegionGranularity(1.);
    gridBack->setPos(0, 0);
    gridBack->setZValue(-1);
    //gridBack->update();
}

void MainWindow::clearGrid()
{
    if (gridBack != nullptr)
        delete gridBack;
}

void MainWindow::onMenu(const QPoint point)
{
    Q_UNUSED(point);

    QCursor cur;

    QMenu menu;
    menu.addAction(setVisibleNum);
    menu.addAction(setVisibleNumFastening);
    menu.exec(cur.pos());
}

void MainWindow::addNumberItem(int m_ID, bool clicked, bool mode)
{
    m_IDitem = m_ID;
    m_doubleClick = clicked;
    changePositionAndRotationEl = mode;
    //обнуляем счетчик на возврат
    currentStep = -1;
    //обнуляем массив
    rendo_changePositionRotation.clear();

    if (changePositionAndRotationEl)
    {
        //необходимо добавить проверку на наличие выбранного элемента в массиве
        if (m_changePositionRotation.size() > 0)
        {
            bool mark = false;//флаг существования выбранного элемента в массиве для перемещения
            for (int u = 0; u < m_changePositionRotation.size(); u++)
                if (m_changePositionRotation.at(u).graphicsItem() == m_itemsListBuff.at(m_ID - 1).graphicsItem())
                    mark = true;
            if (!mark)
                m_changePositionRotation.emplace_back(m_itemsListBuff.at(m_ID - 1));
        }
        else
            m_changePositionRotation.emplace_back(m_itemsListBuff.at(m_ID - 1));
    }
    gViewModule->setFocus();
}

void MainWindow::on_TwCoordBrowser_itemChanged(QTableWidgetItem *item)
{
    if (CanChange)
    {
        unsigned int nColumn = static_cast<unsigned int>(item->column());
        unsigned int nRow = static_cast<unsigned int>(item->row());
        switch (nColumn)
        {
        case COL_X:
            x_Pos.at(nRow) = item->text().toDouble();//x_Pos
            break;
        case COL_Y:
            y_Pos.at(nRow) = item->text().toDouble();//y_Pos
            break;
        case COL_Z:
            z_Pos.at(nRow) = item->text().toDouble();//z_Pos
            break;
        }

        QFile file("coords.txt");

        if (file.open(QIODevice::WriteOnly | QIODevice::Truncate))
        {
            QTextStream writeStream(&file);

            for (unsigned int i = 0; i < x_Pos.size(); i++)
            {
                writeStream << QString::number(x_Pos.at(i)) << "\t"
                            << QString::number(y_Pos.at(i)) << "\t"
                            << QString::number(z_Pos.at(i)) << "\n";
            }
        }
        file.close();
        //Отслеживаем положение объекта и изменянем его для 2d графики
        for (unsigned int i = 0; i < m_itemsListBuff.size(); ++i)
        {
            if (static_cast<unsigned int>(m_itemsListBuff.at(i).Id()) == nRow)
            {
                m_itemsListBuff.at(i).graphicsItem()->setPos(QPointF((x_Pos.at(i) / static_cast<double>(scaleW)
                                                                      + gViewModule->width()/2),
                                                                     (-y_Pos.at(i)/ static_cast<double>(scaleH)
                                                                      + gViewModule->height()/2) ));
                break;
            }
        }
        for (unsigned int i = 0; i < m_objList.size(); i++)
        {
            if (static_cast<unsigned int>(m_objList.at(i).ID()) == nRow)
            {
                //Qt3DCore::QEntity *rEntity = m_objList.at(i).m_rootEntity;
                Qt3DCore::QTransform* transformObj = new Qt3DCore::QTransform();
                transformObj->setTranslation(QVector3D(static_cast<float>(x_Pos.at(i)),
                                                       static_cast<float>(y_Pos.at(i)),
                                                       static_cast<float>(z_Pos.at(i) - 300.)));
                switch (diagIndex) {
                case RAMP:
                    break;
                case CIRCLE:
                    transformObj->setRotationX(90.f);//так как цилиндрические объекты создаются повернутыми
                    break;

                }
                m_objList.at(i).m_rootEntity->addComponent(transformObj);
                break;
            }
        }
        ////Запоминаем текущий шаг
        //createStepMemory();
    }
}

void MainWindow::addValueKey(int ID)
{
    double oneStepX = 0;
    double oneStepY = 0;
    if (m_doubleClick || changePositionAndRotationEl)
    {
        switch (typeKey) {
        case UP:
            oneStepY = +keyStep;
            break;
        case DOWN:
            oneStepY = -keyStep;
            break;
        case RIGHT:
            oneStepX = keyStep;
            break;
        case LEFT:
            oneStepX = -keyStep;
            break;
        }
    }

    //CanChange = false;

    if (!changePositionAndRotationEl)
    {
        CanChange = true;
        //перезаписываем новую кординату
        x_Pos.at(ID - 1) += oneStepX;
        y_Pos.at(ID - 1) += oneStepY;
        //добавляем ее в таблицу
        ui->TwCoordBrowser->setItem(ID - 1, COL_X,
                                    new QTableWidgetItem(QString("%1").arg(x_Pos.at(ID - 1))));
        ui->TwCoordBrowser->setItem(ID - 1, COL_Y,
                                    new QTableWidgetItem(QString("%1").arg(y_Pos.at(ID - 1))));

        m_itemsListBuff.at(ID - 1).setX((x_Pos.at(ID - 1)) / static_cast<double>(scaleW)
                                        + static_cast<double>(deltW / scaleW));
        m_itemsListBuff.at(ID - 1).setY((y_Pos.at(ID - 1)) / static_cast<double>(scaleH)
                                        + static_cast<double>(deltH / scaleH));
        //Запоминаем текущий шаг

        markedChangedItem(ID - 1);
        //при случаи единичного изменения
        if (addedElements)
            realTimechangePosition(ID, oneStepX, -oneStepY);
    }
    else
    {
        //CanChange = false;

        if (m_changePositionRotation.size() > 0)
            for (unsigned int i = 0; i < m_changePositionRotation.size(); i++)
            {
                m_changePositionRotation.at(i).setX((x_Pos.at(m_changePositionRotation.at(i).Id())) / static_cast<double>(scaleW)
                                                    + static_cast<double>(deltW / scaleW) + oneStepX);
                m_changePositionRotation.at(i).setY((-y_Pos.at(m_changePositionRotation.at(i).Id())) / static_cast<double>(scaleH)
                                                    + static_cast<double>(deltH / scaleH) - oneStepY);
                //осуществляем перемещение для всех элементов массива
                m_changePositionRotation.at(i).graphicsItem()->setPos(m_changePositionRotation.at(i).X(),
                                                                      m_changePositionRotation.at(i).Y());

                x_Pos.at(m_changePositionRotation.at(i).Id()) += oneStepX;
                y_Pos.at(m_changePositionRotation.at(i).Id()) += oneStepY;
                //записываем новые координаты в таблицу
                ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_X,
                                            new QTableWidgetItem(QString("%1").arg(x_Pos.at(m_changePositionRotation.at(i).Id()))));
                ui->TwCoordBrowser->setItem(m_changePositionRotation.at(i).Id(), COL_Y,
                                            new QTableWidgetItem(QString("%1").arg(y_Pos.at(m_changePositionRotation.at(i).Id()))));

                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setX(m_changePositionRotation.at(i).X());
                m_itemsListBuff.at(m_changePositionRotation.at(i).Id()).setY(m_changePositionRotation.at(i).Y());

                markedChangedItem(m_changePositionRotation.at(i).Id());
                if (addedElements)
                    realTimechangePosition(m_changePositionRotation.at(i).Id() + 1, oneStepX, -oneStepY);
            }

    }
    //если элементы подгружены необходимо запомнить изменения и произвести перерассчет

}

void MainWindow::drawModuleElements()
{
    float progress = 0.f;
    int NumEl = 0;
    loadBlock = true;
    loadSubElements = true;

    QString m_szFileName = QFileDialog::getOpenFileName(this,
                                                        tr("Файл координат"), "", tr("Txt Files (*.txt)"));
    //вызываем считывание файла
    QFile file(m_szFileName);
    if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
        return;

    double m_iN = 0;
    TableInit();

    x_PosEl.clear();
    y_PosEl.clear();
    z_PosEl.clear();
    x_PosEl3D.clear();
    y_PosEl3D.clear();
    z_PosEl3D.clear();
    xLast.clear();
    yLast.clear();
    zLast.clear();
    buffX.clear();
    buffY.clear();
    tXe.clear();
    tYe.clear();
    tZe.clear();
    rotationArray.clear();
    //очищаем лист подгрузки элементов
    if (m_itemsListE.size() != 0)
        for (unsigned int i = 0; i < m_itemsListE.size(); i++)
            sceneE->removeItem(m_itemsListE.at(i).graphicsItem());
    sceneE->clear();
    m_itemsListE.clear();

    //подсчитываем количество элементов в файле координат
    QTextStream in(&file);
    while (!in.atEnd()) {
        QString line = in.readLine();
        NumEl++;
    }

    file.close();

    if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
        return;
    ui->statusBar->showMessage("Загрузка координат элементов модуля", -1);
    float X, Y, Z;
    for (unsigned int i = 0; i < NumEl; i++)
    {
        in >> X >> Y >> Z;
        x_PosEl.emplace_back(static_cast<double>(X));
        y_PosEl.emplace_back(static_cast<double>(Y));
        z_PosEl.emplace_back(static_cast<double>(Z));
        x_PosEl3D.emplace_back(static_cast<double>(X));
        y_PosEl3D.emplace_back(static_cast<double>(Y));
        z_PosEl3D.emplace_back(static_cast<double>(Z));
        rotationArray.emplace_back(0);
        //заполняем браузер координат
        //		ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));


        progress += 100.0 / (m_iN);

        ui->progressBar->setValue(static_cast<int>(round(progress)));
    }

    //центруем координаты
    MinMaxCoordsCalc(x_PosEl, y_PosEl);
    float CenterW = static_cast<float>(abs(maxX) - abs(minX));//
    float CenterH = static_cast<float>(abs(maxY) - abs(minY));//

    for (unsigned int i = 0; i < x_PosEl.size(); i++)
    {
        x_PosEl.at(i) = x_PosEl.at(i) - std::abs(CenterW / 2);//центрирование сетки координат
        y_PosEl.at(i) = y_PosEl.at(i) - std::abs(CenterH / 2);
    }

    QFile files("coords.txt");

    if (files.open(QIODevice::WriteOnly | QIODevice::Truncate))
    {
        QTextStream writeStream(&files);
        for (unsigned int j = 0; j < m_itemsListBuff.size(); j++)
        {

            reCalcPositionElements(writeStream, j);

            xLast.clear();
            yLast.clear();
        }
        buffX.clear();
        buffY.clear();

        x_PosEl3D.clear();
        y_PosEl3D.clear();
        z_PosEl3D.clear();
    }
    else
        QMessageBox::warning(container, "файл открыт", "Открыт файл для записи координат");
    files.close();
    //для корректного отображения сразу переключаем отображение на нужную вкладку
    ui->tabWidget->setCurrentIndex(ELEMENTS);
    gViewElements->setScene(sceneE);
    //производим отрисовку элементов модулей в отдельном окне
    if(!createElementsDraw(gViewElements, "coords.txt", sceneE, m_itemsListE))
        return;
    //обработка элементов на неактивнгость
    for(int i = 0; i < m_itemsListE.size(); i++)
        m_itemsListE.at(i).graphicsItem()->setEventActive(true);
    //добавление сетки однако из-за большого потребления ресурсов отключено
    // 	clearGrid();
    // 	addGrid(gViewElements, sceneE);
    SetCordsOnCenter(widthModule, heightModule, 0, false, fileXe, fileYe, gViewElements);
    //запоминаем начальное состояние системы
    currentStep = -1;
    m_stepE.clear();
    changedItem.clear();
    fileXe.clear();
    fileYe.clear();
    fileZe.clear();
    //запоминаем навые значения с которыми работаем теперь
    //m_itemsListBuff = m_itemsListE;
    //делаем сцену буфером
    buffScene = sceneE;
    //
    //createStepMemory(m_stepE);
    //учитываем наличие элементов под каждым модулем
    addedElements = true;
    //запоминаем количество элементов
    blockElementsNumber = NumEl;
    //
    loadSubElements = false;
    markedChooseTab(ELEMENTS);
}

bool MainWindow::CalculateCirclePosition()
{
    freeMemory();
    //инициализируем наше отображение антенной решетки
    //обнуляем бар прогресса
    countColor = 6;
    double progress = 0.;
    ui->progressBar->setValue(static_cast<int>(progress));

    //Принимаемы параметры с диалога
    int elementOfRow = dlRow;                                    //количество элементов в строке
    int elementOfColumn = dlCol;                                    //количество элементов в столбце
    int partSin = dlpartSine;
    double A = m_Aheigh;//амплитуда изменения
    int stepBetweenCols = static_cast<int>(dlBetwCollLength);      //расстояние между элементами по строкам
    int stepBetweenRows = static_cast<int>(dlBetwRowLength);      //расстояние между элементами по колоннам
    //проверяем четночть строк, это влияет на правильность

    double even = elementOfRow % 2 == 0 ? 0.5 : 1;
    double set = (90. / abs(partSin - 5)); //тем самым формируем полукруг из элементов

    int numOfElCircle = dlNumOfElCircle;

    double startAngle = static_cast<double>(dlAngle);//начальная точка полукруга синуса

    //задаем отсчет
    int Id = 0;
    float coeff = 0.f;
    float CommonWidth = 0;//смещение блоков по ширине
    float CommonHeigth = 0;// по высоте
    //Создаем массивы под параметры ширины и высоты расположения элементов на сцене
    std::vector<double> xPositionElement;
    std::vector<double> yPositionElement;

    //Расчет коэффициента домнажения для отрисовки элемента
    if (widthModule > heightModule)
        coeff = static_cast<float>(widthModule / heightModule);//пропорция соотношения сторон одного блока
    else
        coeff = static_cast<float>(heightModule / widthModule);

    //передаем в графический виджет сцену
    gViewModule->setRenderHint(QPainter::Antialiasing, true);
    gViewModule->setScene(sceneM);
    gViewModule->scene()->setFocus();

    //задаем ей параметры
    sceneM->setSceneRect(-gViewModule->x(), -gViewModule->y(), gViewModule->width(), gViewModule->height());
    sceneE->setSceneRect(-gViewModule->x(), -gViewModule->y(), gViewModule->width(), gViewModule->height());
    //запоминаем ширину и высоту сцены для последующего центрирования генерируемых координат
    float ScWidth = static_cast<float>(sceneM->width());
    float ScHeight = static_cast<float> (sceneM->height());

    //вычисляем значение для масштабирования
    float scale = (ScWidth / ScHeight) * 4;

    int collH = static_cast<int>(elementOfRow * elementOfColumn  * ((elementOfColumn - ScHeight) / elementOfColumn));//расстояние между элементами
    int collW = static_cast<int>(elementOfRow * elementOfColumn  * ((elementOfRow - ScWidth) / elementOfRow));
    float SquareSc = ((ScWidth * ScHeight) - (collH + collW)) / (elementOfRow * elementOfColumn * scale);//расчет площади одного прямоугольника плюс учет расстояния между элементами

    //рассчитываем размеры рисуемых блоков относитеьно полученных значений
    float ArWidth = sqrtf(SquareSc / coeff);//282.67f;
    float ArHeight = SquareSc / ArWidth;// 326.4f;

    //На основе полученных значений формируем область отрисовки и сам рисуемый объект
    QRect rectP(-static_cast<int>(ArWidth), -static_cast<int>(ArHeight),
                static_cast<int>(ArWidth), static_cast<int>(ArHeight));
    QRectF rectB = QRectF(-static_cast<int>(ArWidth / 2), -static_cast<int>(ArHeight / 2),
                          static_cast<int>(ArWidth), static_cast<int>(ArHeight));
    //запоминаем текущие размеры для рисования элемента
    rectBuff = rectP;
    rectFBuff = rectB;
    //Считаем объем апертуры что будет расположенна на экране с учетом смещения
    for (int i = 0; i < elementOfRow; i++)
        CommonWidth += static_cast<float>(rectB.width());
    for (int i = 0; i < elementOfColumn; i++)
        CommonHeigth += static_cast<float>(rectB.height());
    //учитываем края АР
    //CommonHeigth -= stepBetweenRows;
    //CommonWidth -= stepBetweenCols;
    //Запоминаем центр от которого будем рисовать окружность
    double iStartPointX = static_cast<double>(ScWidth / 2);
    double iStartPointY = static_cast<double>(ScHeight / 2);

    double radius = numOfElCircle > 1 ? numOfElCircle * static_cast<double>(ArWidth + stepBetweenRows) / (2. * PI) : 0.;
    double dphi = 0.0;
    double Phi = 30.;
    int numElement = elementOfRow * elementOfColumn;
    //radius = 0;//(numOfElFirstCircle * ArWidth)/(2*PI);
    Id = 0;
    int i = 0;
    std::vector<double> diagRad;
    diagRad.emplace_back(1);
    diagRad.emplace_back(dlWidthPAA / dlHightPAA);//задаем соотношение сторон
    int k = 0;
    while (i < numElement)
    {
        //Цикл на первую окружность элементы

        dphi = 2. * PI / (numOfElCircle);
        Phi = startAngle * 2 * PI / 180.;//89.7//очень точный параметр
        //считаем угол поворота для каждого элемента
        set = (90. / abs(partSin - 5)); //тем самым формируем полукруг из элементов
        double angle = 360.0 / (numOfElCircle);

        for (int j = 0; j < numOfElCircle; j++)
        {
            //необходимо понять сколько элеменов на первой окружности
            x_Pos.emplace_back((radius * cos(Phi) * ((dlCalcEllipse == true) ? diagRad.at(k) : 1.)));
            y_Pos.emplace_back(radius * sin(Phi));//если сжимаем к вертикали
            z_Pos.emplace_back(0);
            rotationArray.emplace_back(0);

            x_Pos.back() = x_Pos.back() + iStartPointX;
            y_Pos.back() = y_Pos.back() + iStartPointY;

            xPositionElement.emplace_back(x_Pos.back());
            yPositionElement.emplace_back(y_Pos.back());

            createElementPaint(Id, rectP, rectB, setVisibleNum->isChecked(), diagIndex);

            //добавляем в массив-структуру
            m_itemsListM.push_back(List(Id, paintAr, static_cast<float>(xPositionElement.back()), static_cast<float>(yPositionElement.back()), 0.f, false, 0.f));// , x_Pos, y_Pos, z_Pos));
            //передаем на отрисовку
            sceneM->addItem(m_itemsListM.back().graphicsItem());
            //задаем позицию
            m_itemsListM.back().graphicsItem()->setPos(xPositionElement.back(), yPositionElement.back());

            //m_itemsList.back().graphicsItem()->setRotation(45);
            if (dlRotate)
            {
                //ui->gView->itemAt(static_cast<int>(std::round(xPositionElement.back())),static_cast<int>(std::round(yPositionElement.back())))->setRotation(angle*j);
                m_itemsListM.back().graphicsItem()->setRotation(angle*j);
                rotationArray.back() = angle * j;
            }
            //Расчет z позиции элементов
            if (pyramidal)
            {
                if (i < dlNumOfElCircle)
                    z_Pos.back() = A;
                else
                    z_Pos.back() = fabs(A * sin(PI * (startAngle + set) / 180.0));
            }
            //опредиляем угол для возможного количества элементов на первой окружности
            Phi += dphi;
            Id++;
            i++;

            //обновляем статус
            progress += 100.0 / (elementOfRow*elementOfColumn);
            ui->progressBar->setValue(static_cast<int>(round(progress)));
        }
        k++;
        diagRad.emplace_back(1);
        //производим перерасчет элементов
        radius += static_cast<double>(ArHeight) + stepBetweenRows;

        numOfElCircle = radius * 2. * PI / (ArWidth + stepBetweenCols);

        if ((i + numOfElCircle) > numElement)
            numOfElCircle = numElement - i;


    }
    //центрирование координат
    SetCordsOnCenter(ArWidth, ArHeight, elementOfRow, true, x_Pos, y_Pos,gViewModule);
    //рассчет z координаты если выбрана функция
    calculateZCoord();
    //
    currentStep = -1;
    m_stepM.clear();
    changedItem.clear();

    m_itemsListBuff = m_itemsListM;
    buffScene = sceneM;
    //Запоминаем текущий шаг
    createStepMemory(m_stepM);

    //проверяем заполненность таблицы
    TableInit();

    QFile file("coords.txt");

    if (file.open(QIODevice::WriteOnly | QIODevice::Truncate))
    {
        QTextStream writeStream(&file);

        for (unsigned int i = 0; i < x_Pos.size(); i++)
        {
            writeStream << QString::number(x_Pos.at(i)) << "\t"
                        << QString::number(y_Pos.at(i)) << "\t"
                        << QString::number(z_Pos.at(i)) << "\n";

            ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(x_Pos.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(y_Pos.at(i))));
            ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(z_Pos.at(i))));

        }
    }
    file.close();
    //определяем рабочее поле
    markedChooseTab(MODULES);
    ui->tabWidget->setCurrentIndex(MODULES);

    return true;
}

void MainWindow::on_lE_Rotation_textChanged(const QString &arg1)
{
    rotateAngle = arg1.toFloat();
    //rotateElementsOnCentr = true;
    //rotateElementOnCentr = false;
}

void MainWindow::on_cHGrid_toggled(bool checked)
{
    visibleGrid = checked;
    if(checked)
        addGrid(gViewModule, sceneM);
    else
        clearGrid();
    gridBack->update();
}

void MainWindow::on_dsBStep_valueChanged(double arg1)
{
    keyStep = arg1;
}

void MainWindow::on_pBAdedExist_clicked()
{
    if(loadBlock)
    {
        QString m_szFileName = QFileDialog::getOpenFileName(this,
                                                            tr("Файл координат"), "", tr("Txt Files (*.txt)"));

        if (m_szFileName != "")
        {
            countColor++;
            //createElementsDraw(gViewModule, m_szFileName, sceneM, m_itemsListM);
            //gViewModule->fitInView(sceneM->sceneRect(), Qt::KeepAspectRatio);
            //sceneM->setSceneRect(-gViewModule->x(), -gViewModule->y(), gViewModule->width(), gViewModule->height());
            gViewModule->setScene(sceneM);
            //gViewModule->setRenderHint(QPainter::Antialiasing, true);    /// Устанавливаем сглаживание
            //LoadCoordsGview(fileName);
            //вызываем диалог для корректировки размеров модуля
            diagMod = new DialogMod();
            diagMod->initial(areYouOnDarkSide);
            if (!diagMod->exec())
                return;
            diagIndex = diagMod->m_index;
            dlgColor = diagMod->elementColor();
            //после успешного завершения диалога принимаем тип данных
            switch (diagIndex) {
            case RAMP:
                widthModule = diagMod->ModW;
                heightModule = diagMod->ModH;
                elm_Distance = diagMod->m_elDistant;
                break;
            case POLYGON:
                widthModule = heightModule = 2 * diagMod->WidthSide;
                widthSide = diagMod->WidthSide;
                NumSide = diagMod->NumOfSides;
                elm_Distance = diagMod->m_elDistant;
                break;
            case CIRCLE:
                widthModule = heightModule = 2 * diagMod->elRad;
                radiusModule = diagMod->elRad;
                NumSide = diagMod->NumOfSides;
                elm_Distance = diagMod->m_elDistant;
                break;
            }


            double progress = 0.0;

            ui->progressBar->setValue(static_cast<int>(round(progress)));

            QFile file(m_szFileName);
            if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
                return;

            double m_iN = 0;
            //TableInit();

            fileXe.clear();
            fileYe.clear();
            fileZe.clear();
            //заменим
            std::vector<double> Xf,Yf,Zf;
            //x_Pos3D.clear();
            //y_Pos3D.clear();
            //z_Pos3D.clear();

            //подсчитываем количество элементов в файле координат
            QTextStream in(&file);
            while (!in.atEnd()) {
                QString line = in.readLine();
                m_iN++;
            }

            file.close();

            if (!file.open(QIODevice::ReadOnly | QIODevice::Text))
                return;
            ui->statusBar->showMessage("Загрузка координат", -1);
            float X, Y, Z;
            int size = ui->TwCoordBrowser->rowCount();
            for (unsigned int i =size ; i < m_iN + size; i++)
            {
                in >> X >> Y >> Z;
                fileXe.emplace_back(static_cast<double>(X));
                fileYe.emplace_back(static_cast<double>(Y));
                fileZe.emplace_back(static_cast<double>(Z));
                Xf.emplace_back(static_cast<double>(X));
                Yf.emplace_back(static_cast<double>(Y));
                Zf.emplace_back(static_cast<double>(Z));
                x_Pos.emplace_back(static_cast<double>(X));
                y_Pos.emplace_back(static_cast<double>(Y));
                z_Pos.emplace_back(static_cast<double>(Z));
                rotationArray.emplace_back(0);
                //заполняем браузер координат
                ui->TwCoordBrowser->setRowCount(static_cast<int>(i + 1));

                //ui->TwCoordBrowser->setItem(static_cast<int>(i),0,new QTableWidgetItem(QString::number(i+1)));
                ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_X, new QTableWidgetItem(QString("%1").arg(x_Pos.at(i))));//так как отображение перевернуто
                ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Y, new QTableWidgetItem(QString("%1").arg(y_Pos.at(i))));//то загружаем обратные координаты
                ui->TwCoordBrowser->setItem(static_cast<int>(i), COL_Z, new QTableWidgetItem(QString("%1").arg(z_Pos.at(i))));

                progress += 100.0 / (m_iN);

                ui->progressBar->setValue(static_cast<int>(round(progress)));
            }


            file.close();

            CanChange = true;
            ui->statusBar->showMessage("Загрузка завершина", 10);

            MinMaxCoordsCalc(Xf, Yf);
            ////Находим стороны знаимаемые площадью АР ширину и высоту
            //Находим стороны знаимаемые площадью АР ширину и высоту
            double SquareW = std::abs(maxX) - abs(minX);//крайнее левое положение координат по X
            double SquareH = std::abs(maxY) - abs(minY);//крайнее левое положение координат по Y
            //необходимо расчитать размер одного элемента
            float scale = (SquareW / SquareH) * 4;//Возьмем универсальное уменьшение
            double coeff;
            coeffH = 1.0;
            coeffW = 1.0;

            //центрируем координаты на области отображения

            double gVwidth = gViewModule->width() / 2;
            double gVheight = gViewModule->height() / 2;

            coeff = (gViewModule->width()*gViewModule->height()) / (SquareH*SquareW);
            scaleHE = scaleWE = static_cast<float>(coeff);
            //Если площадь экрана меньше площади решетки то необходимо пересчитать координаты элементов для экрана
            bool standartCoeff = false;
            for (unsigned int i = 0; i < Xf.size(); i++)
            {
                Xf.at(i) = Xf.at(i);//уходим от центрирования координат для отображения объектов в виджете/difference
                Yf.at(i) = -Yf.at(i);// /difference
            }
            if (gViewModule->width() < SquareW)
                scaleWE = coeffW = gViewModule->width() / SquareW;
            if (gViewModule->height() < SquareH)
                scaleHE = coeffH = gViewModule->height() / SquareH;
            //если один из коэффициентов не изменен то присваиваем новое значение каждому
            //учитываем условие окружности
            if (widthModule != heightModule)
            {
                if (coeffH == 1.0 || coeffW == 1.0)
                {
                    if (coeffW != 1.0)
                        coeff = coeffW;
                    if (coeffH != 1.0)
                        coeff = coeffH;
                }
                else
                    standartCoeff = true;
            }
            else
            {
                if (coeffH < coeffW)
                    coeffW = coeffH;
                else
                    coeffH = coeffW;
                standartCoeff = true;
            }
            for (unsigned int i = 0; i < Xf.size(); i++)
            {
                Xf.at(i) = (Xf.at(i)*((!standartCoeff) ? coeff : coeffW) + static_cast<double>(gVwidth));// //уходим от центрирования координат для отображения объектов в виджете/difference
                Yf.at(i) = (Yf.at(i) *((!standartCoeff) ? coeff : coeffH) + static_cast<double>(gVheight));////difference
            }
            //соотносим координаты с размером окна

            inFile = true;
            //отрисовываем объекты
            drawInGraphicScene(Xf, Yf, Zf, gViewModule, sceneM, m_itemsListM);
            //clearGrid();
            //gridBack = nullptr;

            //выделяем память
            //        SetCordsOnCenter(rectBuff.width(), rectBuff.height(), 0, true, x_Pos3D, x_Pos3D, gViewModule);
            //        x_Pos = fileXe;
            //        y_Pos = fileXe;
            //        z_Pos = fileXe;
            //        x_Pos = fileXe;
            //        y_Pos = fileYe;
            //        z_Pos = fileZe;

            scaleH = coeffH;
            scaleW = coeffW;
            //deltH = deltH;
            //deltW = deltW;

            m_itemsListBuff = m_itemsListM;

            addGrid(gViewModule, sceneM);

            tabIndex = MODULES;
            m_objects.emplace_back(Xf.size(),widthModule,heightModule,diagIndex,NumSide,dlgColor);
        }
    }
}

void MainWindow::openSettingsWindow()
{
    QSettings geomMasterSettings(ORGANIZATION_NAME, APPLICATION_NAME);
    SettingsDialog* sDlg = new SettingsDialog(areYouOnDarkSide,dlgIsOpen,colorList->color());
    if(!sDlg->exec())
        return;
    areYouOnDarkSide = sDlg->areYoudarkSide;
    dlgIsOpen = sDlg->dlgIsOpen;
    gViewModule->setBackgroundBrush(sDlg->fonColor());
    colorList->setColor(sDlg->fonColor());

    if(areYouOnDarkSide)
    {
        QFile file(":/styles/darkStyle.qss");
        if(!file.open(QIODevice::ReadOnly))
            return;
        QString stylesheet = QString::fromUtf8(file.readAll());
        this->setStyleSheet(stylesheet);
        ui->statusBar->showMessage("It's all for you, My Master", -1);
        //gViewModule->setBackgroundBrush(QColor("white"));
    }
    else
    {
        QFile file(":/styles/lightStyle.qss");
        if(!file.open(QIODevice::ReadOnly))
            return;
        QString stylesheet = QString::fromUtf8(file.readAll());
        this->setStyleSheet(stylesheet);
        ui->statusBar->clearMessage();
        //gViewModule->setBackgroundBrush(QColor("transparant"));
    }
    //запоминаем параметр
    geomMasterSettings.setValue(OPEN3DDLG,dlgIsOpen);
    geomMasterSettings.setValue(DARKTEME,areYouOnDarkSide);

}

void MainWindow::openAboutWindow()
{
    UniversalDialog* dlg = new UniversalDialog(ABOUT, this);
    if(!dlg->exec())
        return;
    //QMessageBox::warning(this,"Предупреждение","Сбой работы окна");
}

void MainWindow::openHelpWindow()
{
    UniversalDialog* dlg = new UniversalDialog(HELP,this);
    if(!dlg->exec())
        return;
    //QMessageBox::warning(this,"Предупреждение","Сбой работы окна");
}

void MainWindow::openOnFile(QString str_Name)
{
    countColor = 0;//сбрасываем счетчик
    freeMemory();
    //sceneM->setSceneRect(-3,-3,1628,876);
    gViewModule->setScene(sceneM);
    gViewModule->fitInView(sceneM->sceneRect(), Qt::KeepAspectRatio);
    QRectF sceneRec = sceneM->sceneRect();

    if(!createElementsDraw(gViewModule, str_Name, sceneM, m_itemsListM))
        return;

    //clearGrid();
    gridBack = nullptr;
    typeIndex = diagIndex;
    //выделяем память
    SetCordsOnCenter(rectBuff.width(), rectBuff.height(), 0, true, fileXe, fileYe, gViewModule);
    x_Pos = x_Pos3D;
    y_Pos = y_Pos3D;
    z_Pos = z_Pos3D;

    m_itemsListBuff = m_itemsListM;

    addGrid(gViewModule, sceneM);

    tabIndex = MODULES;
    ui->statusBar->showMessage("Рассчет окончен", -1);
}
