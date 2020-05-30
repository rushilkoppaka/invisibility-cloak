# invisibility-cloak
creating the famous Harry Potter invisibility cloak with opencv python
```python
import cv2
import numpy as np

cap = cv2.VideoCapture(0)
fourcc = cv2.VideoWriter_fourcc(*'XVID')
out = cv2.VideoWriter('output.avi', fourcc, 20.0, (640, 480))

background_image= cv2.imread('background image.jpg')

while (cap.isOpened()):

    ret, frame = cap.read()
    if ret == True:
        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

        LB=np.array([80,144,0])                     #blue
        UB=np.array([125,255,172])                  #blue

        #LB = np.array([lb,lg,lr])
        #UB = np.array([ub,ug,ur])

        mask=cv2.inRange(hsv,LB,UB)

        kernel= np.ones((5,5),np.uint8)
        mask3=cv2.morphologyEx(mask,cv2.MORPH_OPEN,kernel,iterations=1)

        frame1=frame.copy()
        frame2=background_image.copy()

        frame1[np.where(mask3==255)]=0
        frame2[np.where(mask3==0)]=0

        res= cv2.add(frame1,frame2)

        res2= cv2.medianBlur(res,5)
        res2= cv2.GaussianBlur(res2,(5,5),0)
        res2= cv2.medianBlur(res,7)

        cv2.imshow('invisibility cloak', res2)
        #cv2.imshow('mask',mask3)

        out.write(res2)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
    else:
        break

cap.release()
out.release()
cv2.destroyAllWindows()

```
