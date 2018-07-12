import pygame
pygame.init()
tamaño_pantalla = ancho,alto = 612,391
display=pygame.display
pantalla = display.set_mode((tamaño_pantalla))
game_name=display.set_caption('Futbol asi bien prron')

done = False
reloj = pygame.time.Clock()
pygame.key.set_repeat(1, 10)
presionado = pygame.key.get_pressed()
#---------------------------------------------------------------------------------------------
class cancha(pygame.sprite.Sprite):
	def __init__(self):
		pygame.sprite.Sprite.__init__(self)
		self.image=pygame.image.load('cancha.jpg')
		self.rect=self.image.get_rect()
#---------------------------------------------------------------------------------------------
class jugadores(pygame.sprite.Sprite):
	import pygame 
	def __init__(self,x,y,r,g,b):
		pygame.sprite.Sprite.__init__(self)
		#posiciones actuales(las que cambian)
		self.x=x
		self.y=y
		#sus puntos especiales
		self.collision = [False] * 9
		#tamaño de los jugadores
		self.ancho=14
		self.alto=35
		#representaciones visuales de los jugadores
		self.color=(r,g,b)
		self.image = pygame.Surface((self.ancho, self.alto))
		self.image.fill((r,g,b))
		self.rect=self.image.get_rect(topright=(self.x,self.y))
		
	def mover_derecha(self):
		#mueve la palanca de la derecha
		presionado = pygame.key.get_pressed()
		if presionado[pygame.K_w] and self.y != 0:
			self.mover(0,-10)
		else:
			pass
		if presionado[pygame.K_s] and self.y < 355:
			self.mover(0,10)
		else:
			pass
		if presionado[pygame.K_d] and self.x < 612:
			self.mover(10,0)
		else:
			pass
		if presionado[pygame.K_a] and self.x > 12:
			self.mover(-10,0)
		else:
			pass
		
	def mover_izquierda(self):
		#mueve la palanca de la izquierda
		presionado = pygame.key.get_pressed()
		if presionado[pygame.K_UP] and self.y != 0:
			self.mover(0,-10)
		else:
			pass
		if presionado[pygame.K_DOWN] and self.y < 355:
			self.mover(0,10)
		else:
			pass
		if presionado[pygame.K_RIGHT] and self.x < 612:
			self.mover(10,0)
		else:
			pass
		if presionado[pygame.K_LEFT] and self.x > 12:
			self.mover(-10,0)
		else:
			pass
			
	def mover(self,x,y):
		#mueve la pelota de acuerdo 
		#a los valores que se consiguen arriba
		#(Esta linea tiene que ser en ingles)
		#(para que funcione, perdon)
		self.rect=self.rect.move(x,y)
		
	def revisar_collision(self, rect):
	#esto esta bastante fumado pero dejame explicar
	#asigna 9 puntos en cada cuadro
	#cuando la pelota choca contra alguno, el 
	#jugador le avisa y ella bota de acuerdo
		self.collision[0] = rect.collidepoint(self.rect.topleft)
		self.collision[1] = rect.collidepoint(self.rect.topright)
		self.collision[2] = rect.collidepoint(self.rect.bottomleft)
		self.collision[3] = rect.collidepoint(self.rect.bottomright)
		self.collision[4] = rect.collidepoint(self.rect.midleft)
		self.collision[5] = rect.collidepoint(self.rect.midright)
		self.collision[6] = rect.collidepoint(self.rect.midtop)
		self.collision[7] = rect.collidepoint(self.rect.midbottom)
		self.collision[8] = rect.collidepoint(self.rect.center)
#---------------------------------------------------------------------------------------------	
class pelota(pygame.sprite.Sprite):
	def __init__(self, r,g,b, ancho1, alto1,x,y):   
		pygame.sprite.Sprite.__init__(self)
		#tamaño de la pelota
		self.ancho = ancho1
		self.alto = alto1
		#posicion xy de la pelota
		self.x = x
		self.y = y
		#la representacion visual de la pelota
		self.image = pygame.Surface([self.ancho, self.alto])
		self.image.fill((r,g,b))
		self.rect=self.image.get_rect(topright=(self.x,self.y))
		#velocidad de la pelota en cada eje
		self.dx = 0
		self.dy = 0
		self.velocidad = [self.dx,self.dy]
		
	def reset(self): #esta pone la pelota en su posicion inicial
		self.x, self.y =  309,180
		self.dx, self.dy = 0,0
		self.asignar_velocidad()
		self.rect=self.image.get_rect(topright=(self.x,self.y))
		
	def movimiento(self,players,goal_R,goal_L):
		#esta me revisa unas listas de las porterias
		#y los jugadores y hace cosas si pasa algo
		#(tambien rebota de las orillas xd)
		if len(pygame.sprite.spritecollide(self,players,False)) > 0:
			self.rebotar(jugador_derecha,jugador_izquierda)
		if len(pygame.sprite.spritecollide(self,goal_R,False)) > 0:
			puntos.elevar_puntos(puntaje,'derecha')
			self.reset()
		if len(pygame.sprite.spritecollide(self,goal_L,False)) > 0:
			puntos.elevar_puntos(puntaje,'izquierda')
			self.reset()
		if self.x < 11 or self.x > 605:
			self.rebotar(jugador_derecha,jugador_izquierda)
		if self.y < 11 or self.y > 380:
			self.rebotar(jugador_derecha,jugador_izquierda)
		else:
			pass
			
	def mover(self): #le avisan como moverse y el se mueve
		self.rect = self.rect.move(self.velocidad)
	
	def asignar_velocidad(self): 
	#pos cy, la pelota se mantiene en movimiento
	#y este men controla la velocidad
	#(solo avisale de cualquier cambio)
		self.velocidad = [self.dx,self.dy]
	
	def rebotar(self,derecha1,izquierda1):
		#esta hace que la pelota bote
		#(aun no la acabo jiji)
		if len(pygame.sprite.spritecollide(self,derecha1,False)) > 0:
			derecha.revisar_collision(self.rect)
			if derecha.collision[0] or derecha.collision[2] or derecha.collision[4]:
				self.dx, self.dy = -5,0
				self.asignar_velocidad()
				self.mover()
			if derecha.collision[1] or derecha.collision[3] or derecha.collision[5]:
				self.dx, self.dy = 5,0
				self.asignar_velocidad()
				self.mover()
			if derecha.collision[6]:
				self.dx, self.dy = 0, -5
				self.asignar_velocidad()
				self.mover()
			if derecha.collision[7] or derecha.collision[8]:
				self.dx, self.dy = 0, 5
				self.asignar_velocidad()
				self.mover()
		if len(pygame.sprite.spritecollide(self,izquierda1,False)) > 0:
			izquierda.revisar_collision(self.rect)
			if izquierda.collision[0] or izquierda.collision[2] or izquierda.collision[4]:
				self.dx, self.dy = -5,0
				self.asignar_velocidad()
				self.mover()
			if izquierda.collision[1] or izquierda.collision[3] or izquierda.collision[5]:
				self.dx, self.dy = 5,0
				self.asignar_velocidad()
				self.mover()
			if izquierda.collision[6]:
				self.dx, self.dy = 0,-5
				self.asignar_velocidad()
				self.mover()
			if izquierda.collision[7] or izquierda.collision[8]:
				self.dx, self.dy = 0,5
				self.asignar_velocidad()
				self.mover()
		#Se y estoy conciente de la monstruosidad que acabo de crear
		#ahora solo dios sabe como funciona, pero ojala todo bien
		#perdoname por mi pecado
#---------------------------------------------------------------------------------------------
class porteria(pygame.sprite.Sprite):
	def __init__(self,x,y,r,g,b):
		pygame.sprite.Sprite.__init__(self)
		#posiciones
		self.x=x
		self.y=y
		#tamaños
		self.ancho=10
		self.alto=80
		#ellas mismas
		self.color=(r,g,b)
		self.image=pygame.Surface((self.ancho,self.alto))
		self.image.fill(self.color)
		self.rect=self.image.get_rect(center=(self.x,self.y))
#---------------------------------------------------------------------------------------------
class puntos(pygame.sprite.Sprite):
	#esta mide los puntos
	def __init__(self):
		pygame.sprite.Sprite.__init__(self)
		self.x=306
		self.y=15
		self.ancho=100
		self.alto=20
		self.r,self.g,self.b=255,255,255
		self.color=(self.r,self.g,self.b)
		self.image=pygame.Surface((self.ancho,self.alto))
		self.image.fill(self.color)
		self.rect=self.image.get_rect(center=(self.x,self.y))
	
	def elevar_puntos(self,which):
	#el que se nombra es al que le metieron gol
		if which=='izquierda':
			self.x += 50
			self.r,self.g,self.b=255,0,0
			self.color=(self.r,self.g,self.b)
			self.image=pygame.Surface((self.ancho,self.alto))
			self.image.fill(self.color)
			self.rect=self.image.get_rect(center=(self.x,self.y))
		if which=='derecha':
			self.x -= 50
			self.r,self.g,self.b=0,0,255
			self.color=(self.r,self.g,self.b)
			self.image=pygame.Surface((self.ancho,self.alto))
			self.image.fill(self.color)
			self.rect=self.image.get_rect(center=(self.x,self.y))
#---------------------------------------------------------------------------------------------			
class esclavo:	
	def __init__(self):
		self.name='aqui es para cosas que uso con todo'
		
	def revisar_limites(self):
	#esta revisa cuando alguien se ha salido del campo
	#y los devuelve todos a sus posiciones iniciales
		b=cancha.rect.contains(self.rect)
		if b == False:
			self.reset()
			return True
#---------------------------------------------------------------------------------------------
derecha=jugadores(60,170,255,0,0) #Coordenadas x,y-valores RGB
izquierda=jugadores(550,170,0,0,255) #Coordenadas x,y-valores RGB
derecha_porteria=porteria(27,196,255,0,0) #Coordenadas x,y-valores RGB
izquierda_porteria=porteria(587,196,0,0,255) #Coordenadas x,y-valores RGB
pelota1=pelota(0,0,0,10,10,309,180) #Valores RGB - y sus medidas
cancha=cancha()
puntaje=puntos()
lista_de_lo_visible=pygame.sprite.RenderUpdates(cancha,derecha_porteria,izquierda_porteria,derecha,izquierda,puntaje,pelota1)
#este de arriba es una lista de todo lo visible en el juego
#lo uso para ponerlos en la pantalla 
lista_de_jugadores=pygame.sprite.Group(derecha,izquierda) # Donde 			
porteria_izquierda=pygame.sprite.GroupSingle(izquierda_porteria) 		
porteria_derecha=pygame.sprite.GroupSingle(derecha_porteria)# la Pelota 
jugador_derecha=pygame.sprite.GroupSingle(derecha)
jugador_izquierda=pygame.sprite.GroupSingle(izquierda)# puede rebotar	

while not done:
	for event in pygame.event.get():
		if event.type == pygame.QUIT:
			done = True
		izquierda.mover_izquierda()
		derecha.mover_derecha()
		if puntaje.x < 0:
			raise SystemExit ('Gano Azul!!!')
		if  puntaje.x > 615:
			raise SystemExit ('Gano Rojo!!')
	pelota1.movimiento(lista_de_jugadores,porteria_derecha,porteria_izquierda)
	pelota1.mover()
	esclavo.revisar_limites(pelota1)
	pygame.display.update(lista_de_lo_visible.draw(pantalla)) #actualiza la pantalla
	reloj.tick(120) #hace que esto pase 120 veces xSegundo