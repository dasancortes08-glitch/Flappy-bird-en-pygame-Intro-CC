import pygame
import random
import sys

# ----------------------------
# Configuración inicial
# ----------------------------
pygame.init()

ANCHO, ALTO = 400, 600
pantalla = pygame.display.set_mode((ANCHO, ALTO))
pygame.display.set_caption("Flappy Bird - Simple")
reloj = pygame.time.Clock()
FPS = 60

# Colores
CIELO = (113, 197, 207)
VERDE_TUBO = (60, 179, 90)
VERDE_TUBO_BORDE = (34, 120, 60)
AMARILLO = (255, 213, 79)
NARANJA = (230, 126, 34)
BLANCO = (255, 255, 255)
NEGRO = (30, 30, 30)
SUELO_COLOR = (222, 184, 135)

# ----------------------------
# Parámetros del juego
# ----------------------------
GRAVEDAD = 0.5
FUERZA_SALTO = -8.5
VELOCIDAD_TUBOS = 3
ESPACIO_TUBOS = 150
ANCHO_TUBO = 60
INTERVALO_TUBOS = 90  # frames entre tubos nuevos
ALTO_SUELO = 60


class Pajaro:
    def __init__(self):
        self.x = 80
        self.y = ALTO // 2
        self.radio = 16
        self.velocidad = 0

    def saltar(self):
        self.velocidad = FUERZA_SALTO

    def actualizar(self):
        self.velocidad += GRAVEDAD
        self.y += self.velocidad

    def dibujar(self, superficie):
        centro = (int(self.x), int(self.y))
        pygame.draw.circle(superficie, AMARILLO, centro, self.radio)

    def get_rect(self):
        return pygame.Rect(self.x - self.radio, int(self.y) - self.radio,
                            self.radio * 2, self.radio * 2)


class Tubo:
    def __init__(self, x):
        self.x = x
        self.alto_hueco = random.randint(120, ALTO - ALTO_SUELO - 120 - ESPACIO_TUBOS)

    def actualizar(self):
        self.x -= VELOCIDAD_TUBOS

    def dibujar(self, superficie):
        rect_sup = pygame.Rect(self.x, 0, ANCHO_TUBO, self.alto_hueco)
        pygame.draw.rect(superficie, VERDE_TUBO, rect_sup)
        pygame.draw.rect(superficie, VERDE_TUBO_BORDE, rect_sup, 4)

        y_inf = self.alto_hueco + ESPACIO_TUBOS
        alto_inf = ALTO - ALTO_SUELO - y_inf
        rect_inf = pygame.Rect(self.x, y_inf, ANCHO_TUBO, alto_inf)
        pygame.draw.rect(superficie, VERDE_TUBO, rect_inf)
        pygame.draw.rect(superficie, VERDE_TUBO_BORDE, rect_inf, 4)

    def get_rects(self):
        rect_sup = pygame.Rect(self.x, 0, ANCHO_TUBO, self.alto_hueco)
        y_inf = self.alto_hueco + ESPACIO_TUBOS
        rect_inf = pygame.Rect(self.x, y_inf, ANCHO_TUBO, ALTO - ALTO_SUELO - y_inf)
        return rect_sup, rect_inf


def dibujar_suelo(superficie):
    rect = pygame.Rect(0, ALTO - ALTO_SUELO, ANCHO, ALTO_SUELO)
    pygame.draw.rect(superficie, SUELO_COLOR, rect)
    pygame.draw.rect(superficie, (150, 111, 51), (0, ALTO - ALTO_SUELO, ANCHO, 6))


def reiniciar():
    pajaro = Pajaro()
    tubos = [Tubo(ANCHO + 100)]
    return pajaro, tubos, 0  # pajaro, tubos, contador_frames


def main():
    pajaro, tubos, contador_frames = reiniciar()

    while True:
        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if evento.type == pygame.KEYDOWN and evento.key == pygame.K_SPACE:
                pajaro.saltar()
            if evento.type == pygame.MOUSEBUTTONDOWN:
                pajaro.saltar()

        pajaro.actualizar()

        contador_frames += 1
        if contador_frames >= INTERVALO_TUBOS:
            tubos.append(Tubo(ANCHO + 20))
            contador_frames = 0

        for tubo in tubos:
            tubo.actualizar()

        tubos[:] = [t for t in tubos if t.x > -ANCHO_TUBO - 10]

        # Colisiones -> si choca, se reinicia de una vez (sin pantalla de game over)
        rect_pajaro = pajaro.get_rect()
        choco = False
        if pajaro.y + pajaro.radio >= ALTO - ALTO_SUELO or pajaro.y - pajaro.radio <= 0:
            choco = True
        for tubo in tubos:
            rect_sup, rect_inf = tubo.get_rects()
            if rect_pajaro.colliderect(rect_sup) or rect_pajaro.colliderect(rect_inf):
                choco = True

        if choco:
            pajaro, tubos, contador_frames = reiniciar()

        # Dibujado
        pantalla.fill(CIELO)
        for tubo in tubos:
            tubo.dibujar(pantalla)
        dibujar_suelo(pantalla)
        pajaro.dibujar(pantalla)

        pygame.display.flip()
        reloj.tick(FPS)


if __name__ == "__main__":
    main()
