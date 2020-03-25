AZTEN CHALLENGE PUR120 C64 BASIC GAME - By Arturo Dente

Making my previous 10liner, TENDRIVE120, I made the code to build a mountain a little at a time. Impossible to not recognize the ziqqurat from the old beloved AZTEC CHALLENGE.
So, the idea to recycle that code and to build around it a clone of the first level of AZTEC CHALLENGE.
This is how AZTEN CHALLENGE was born, no bees and flowers, so.

In this game you have:

1) the ziqqurat growing a little at a time
2) the aztec guy running with petscii animation (each frame contained in 4 cells of a string array)
3) a sprite of a arrow coming from 4 directions: up / down, left / right and their combination
4) a sound effect when the arrow has been thrown (yes, there is also sound!)
5) difficulty slightly increasing while playing, making arrow start from nearer the aztec.
6) an energy meter, when the arrow hits you, your energy decreases. If you are hit, better to make the right move anyway, because the arrow is a bit slow (due to c64 basic slowness), so you can result hit twice.

-----------------------------------------------------------

*THE CODE*

	*General thoughts*
Some treat and triks :
	- DATA are scattered everywhere there's a space to do it.
	- the arrow sprite is only three bytes set to 255, so i make a FOR cycle to set 0 all bytes in the sprite area then the 3 to 255.
	- Aztec animation is made in petscii art, the frames are contained in a string array, arranged by 4 cells for every frame.
	- When possible, I used ON ... instead of IF in order to obtain a line not logically breaked by the IF lacking of ELSE

	*Initialization parts*

LINE 0 (and 9)
	Line 0 Code: 0i=54272:w=200:ww=222:y=w:c=0:c$="{reverse on}  {reverse off}":u$=" ":dIa$(16):d$="{down}":r$="{right}":fOt=1to6:u$=u$+u$:d$=d$+d$:r$=r$+r$:nE:gO9

	Explanation: Variable i is intended for sound manipulation, w and ww are only shortcuts for their values. w is the base for arrow y, infact we have y=w immediately after. With w we can always come back to the "normal" y, while y is modified by the program.c$ is a full space, u$ is a empty space, d$ is a "down" char, r$ is a "right" char. These short strings became long with the for immediately after and can be used to make strings of n chars from each of them, using LEFT$. After the FOR cycle there's a goto 9 that is explained  here.

	Line 9 Code: 9v=53248:pOv+33,7:pOv+21,0:pOi+24,15:pOi+6,215:?"{clear}{brown}azten challenge{return}{return}hs:"hs:q=56320:o=65520:b=-1:reS:on2+(pE(q)=127)gO9,1
	
	Explanation: v is the well known variable for sprite's stuff, I use this also for screen background color. Nothing serious here, only initialization of variables. q is used for joystick peeking, restore is used for restoring DATAs and the ON - GOTO in the end waits for a joystick movement to start the game going to line 1. In facts, if peek(q)=127  the expression gives 1, else 2.

	Line 1 Code: 1x=0:?"{clear}":dA" {black}{reverse off}Q","{pink}{188}{reverse on}{orange} {reverse off}{pink}{191}"," {reverse on}{orange}{178}"," {orange}{181}"," {black}{reverse off}Q","{reverse on}{pink}{191}{orange} {reverse off}{pink}{190}"," {reverse on}{orange}{178}"," {orange}{182}","  "," {black}{185}","{pink}{188}{reverse on}{orange}{178}{reverse off}{pink}{190}"," {reverse on}{orange}{125}"," {black}{reverse off}Q","{reverse on}{pink}{191}{orange} {reverse off}{pink}{191}"

	Explanation: nothing, only data. Here there are quite all the DATA needed for the animation strip of the aztec. It will occupy 4x4 cells of the array we will build, 4 states of animation, each of them 4 characters tall.


	Line 2 Code: 2fort=itoi+24:pOi,0:next:ht=8:g=0:h=320:e=0:l=3:fOt=1to16:rEs$:a$(t)=s$+"{reverse off} ":nE:f=1:dA" {reverse on}{orange}{178}"," {pink}{184}"


	Explanation: the first FOR is for initializing SID registers to zero. g and h are two variables whom variation is used to make the arrow starting each turn a little further away from the edges of the screen. So, at the beginning, if the arrow comes from left, x is zero, but after some time x will be - say - 10, so the game will be a little more difficult. Variable l is intended for "lives".  After this, we have finally the FOR cycle to build the array for the animation strip

	Line 3 Code: 3fOs=0to11:?"{home}{blue}{reverse on}"leF(d$,s)leF(u$,40);:nE:pOv+21,1:pO2040,192:d=12288:fOt=dtot+62:pOt,-255*(t<d+3):nE:pOv+39,0:pOv+29,0

	Explanation: we use for the first time the string variables to move on screen via LEFT$. The FOR cycle prints 11 rows in reversed spaces. The second part of the line set the arrow'ss sprite by putting zero in all its registers but not the first 3. In fact, -255*(t<d+3)=255 if and only if we are iterating the first three bytes,else 0.


	Line 4 Code: 4j=pE(q):p=(-(b+2)*(j=127))-3*(j=125)-4*(j=111):p=p-(p=0):a=4*p-3:fOs=atoa+3:?"{home}{yellow}"leF(d$,18+s-a)leF(r$,19)a$(s):nE


	Explanation: this is the first line of the main game cycle. We read joystick and set the right frame according to joystick (if changed) or - if in idle state - alternating the 2 frames of running animation. It's a bit complicated, so let's explain well. We read joystick value in j, and b is a boolean variable alternating itself in every cycle, so it's true, then false,then true, then.... What does it mean? it mean that if joystick is idle (say j=127) , variable p is set to 1 or 2, depending on b. If, else, joystick is in down position we will have p=3, and if fire is pressed we have p=4. Do you get it? p is the position of the frame to print! 1 and 2 are the running frames,3 is when the aztec is bowing his head, and 4 is when he is jumping. And what if the joystick has moved in another direction? if it happens, p would be 0 , but we want a valid number - say 1 - as default, so p=p-(p=0) does the job. Then, using the rect-in-2-points-equation we calculate the position of the first array cell of the frame. Remember, each frame is composed by 4 cells. So , if p=1 (first frame) we start from a=1, if p=2 (second frame) we starts from position a=5, etc, etc.
Finally, we print the 4 arrays cells (from "a" to a+3) in the right position, covering the previous ones.


	Line 5 Code: 5b=nOb:on-(c=0orm=-1)goS8:c=c+1:c=-c*(c<40):f=f-(c=0):pOv+29,-(e>0aNxx>g):g=g-5*(c=0):h=h+5*(c=0):iff=13tH?"{home}winner!":eN


	Explanation: after changing the "b" state (who is described in line 4), we decide if waste some precious time updating energy status and ziqqurat (that are covered in line 8). For this sake, we update these prints when aztec is hit (m=-1, see after) or when we have completed a cycle, that is governed by the c variable, that is turned on 0. After this, we increment c and we test if c is under 40 (the number of cycles we want until the next ziqqurat upgrade). With c=-c*(c<40) we obtain that if c=40, c become 0. Variable f is the score and it's incremented every time c become 0, so every cycle. This line is very rich, because we now have also the arrow'ss visibility toggle, determined by the condition -(e>0aNxx>g). The three variables in this condition are discussed later. Anyway, "e" is related to arrow'ss state, and g is the actual left bound. We should have included also the other bound, but there were no room, and nobody notices it if we don't tell nothing! xx is a sort of x position of the arrow, but it's some more, we will see. We have then  g=g-5*(c=0):h=h+5*(c=0), the left and the right bound the arrow starts from, incrementing by 5 every new cycle of 40. Finally, we test if the score is 13 (so 13 full cycles succesfull, and the ziqqurat arrived to the sky..ehr..up of the monitor), and if it's true, then you are the aztec of the year.


	Line 6 Code: 6k=((-1)^e=-1):y=-w*k-ww*(nOk):x=x-25*(e=1ore=2)+25*(e>2):xx=-x*(x>gaNx<h):ifc=20tHe=1+int(rN(.)*4):x=-h*(e>2):pOi+4,129

	Explanation: k is a variable that is true (-1) if e is odd, else is false (0). Why this? well, I decided that e=0:no arrow. e=1: arrow from left up. e=2 arrow from left dwn. e=3 and 4, same on right. So, when e=1 or e=3 , the arrow is arriving from some direction but anyway from up. In short, k=-1 if and only if the arrow is arriving to the head of the aztec, else k=0. Said this, the arrow's y is
w (whose value is 200 as per row 0) if k (so if arrow up), else arrow's y is ww (that is 222 as per line 0) if not k (so if arrow down). Then, we update the arrow's x making it greater or smaller depending on arrow's provenance. Finally we have the xx variable, as stated before. xx is a x with a bit of neurons, it keeps in mind if x is becoming too big (bigger than the bound h) or too small (smaller then the g bound) and sets itself to 0 in everyone of these two cases. The last if sets arrow's status "e" randomly when we are at half the street of current cycle of 40. If this is the case, we also update x to the bound on the right if e>2 (which determines the 2 states of arrow coming from right). If e<=2 then we have x=0. Wanting to be precise, we should have been started from g, not from 0, but we  trust in the subsequent adjustaments. After this, we starts the arrow's sound with sid.

	Line 7 Code: 7pOv,xxaN255:pOv+16,1+(xx<256):pOv+1,y:m=(pE(v+31)=1):pOi+1,16:pOi,195:pOi+4,16:hs=-hs*(hs>=f)-f*(hs<f):on2+(l<=0)gO9,4


	Explanation: pOv,xxaN255 is the well known sprite rendering-with-an-eye-to-x-when-goes-after-the-255-bound, and the pOv+16,1+(xx<256) is his twin, as v+16 must be set to 1 in order to start to visualize the sprite after the 255th pixel. It's a pain in the axis for us c64 programmers. We then update sprite y and we update m with a sprite vs char collision value: m=(pE(v+31)=1). Given that the only chars in the arrow's path are the aztec ones, if there is a collision then there is an aztec with a headache. Pokes with "i" are sid related. hs is the hiscore variable. Finally we test if lives (or energy, call it as you want) are ended and we decide to restart the cycle or to go to the opening screen accordingly


	Line 8 Code: 8l=l+(m=-1)/2:?"{home}   ":fOz=1tol:pO1023+z,81:nE:t$="{brown}":fOz=1tof:t$=t$+c$:pO780,0:pO781,11+z-f:pO782,20-z:sYo:?t$;:nE:reT


	Explanation: as stated before, we update the energy meter and the ziqqurat. We have energy slighted decreased (0.5 not 1), so when printing the balls of energy we approximate to integer. The ziqqurat is printed using a printat call, and is tall "f" blocks, where f is the current score, the current cycle being played.

That's all, bye.
Arturo Dente (Francesco Clementoni)


