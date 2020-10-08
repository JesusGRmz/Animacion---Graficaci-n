# Animacion---Graficaci-n
Código en el cual se crea una imagen a partir de un numero hexadecimal, después de crearse este se mueve aleatoriamente por la ventana creada sin salirse de ella, la figura hace 10 choques y termina la ejecución del programa


package Unidad1;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Container;
import java.awt.Graphics;
import javax.swing.JFrame;
import javax.swing.JPanel;

public class Animacion extends JPanel {

    // ventana
    private JFrame ventana;
    //contenedor
    private Container contenedor;
    private int AnchoVentana = 800, AltoVentana = 600;

    //Hexadecimal de la figura
    private final int[] FIGURA = {
//        //ejemplo calamardo
//                0x000ff800,
//                0x00300600,
//                0x00400100,
//                0x00800080,
//                0x00800080,
//                0x01000040,
//                0x01000040,
//                0x01063040,
//                0x01083040,
//                0x01108440,
//                0x0092a480        
//       
        //ejemplo fantasma
        0x0007c000,
        0x00183000,
        0x00200800,
        0x00400400,
        0x00540200,
        0x00943900,
        0x00940900,
        0x00800a00,
        0x00aa1100,
        0x00be0100,
        0x00be0100,
        0x00df0100,
        0x00550200,
        0x00200400,
        0x00181800,
        0x0007e000
            //bomba
//        0x00078000,
//        0x001fe000,
//        0x003ff000,
//        0x003ffb00,
//        0x007ffd00,
//        0x0075fd00,
//        0x00f5fd00,
//        0x00f5f900,
//        0x00fff900,
//        0x00fffd00,
//        0x007ffd00,
//        0x007ffb00,
//        0x003ff000,
//        0x00478800,
//        0x00249000,
//        0x00186000
    //bala
//                0x00000000,
//                0x0003f300,
//                0x000d0c00,
//                0x003dfd00,
//                0x0077ff00,
//                0x0067ff00,
//                0x00a7ff00,
//                0x00879f00,
//                0x00cf8f00,
//                0x00fc8f00,
//                0x00780f00,
//                0x00787f00,
//                0x007fff00,
//                0x000fff00,
//                0x0003f300,
//                0x00000000
    //Tanque
//        0xf000000f,
//        0x3000000c,
//        0xbffffffd,
//        0xfff81fff,
//        0xfe17e87f,
//        0xf1ec378f,
//        0xbfec37fd,
//        0x302ff40c,
//        0xf05e3a0f,
//        0x00bc1d00,
//        0x00bb6d00,
//        0x017b6e80,
//        0x01788e80,
//        0x01780e80,
//        0x017d5e80,
//        0x01ffff80
    };
    
    //mascara
    private final int MASCARA = 0x0000001;

    //ancho de bits
    private final int ANCHO_BITS = 32;

    //coordenadas
    private int coordenada_x, coordenada_y, tiempofinal = 5;

    //declarar hilo
    private Thread hilo;

    public Animacion() {
        //inicializar la ventana
        ventana = new JFrame("Dibujando icono");
        //definir tamaño a ventana

        ventana.setSize(AnchoVentana, AltoVentana);
        ventana.setVisible(true);
        ventana.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        ventana.setResizable(false);
        ventana.setLocationRelativeTo(null);

        //Configurando contenedor
        contenedor = ventana.getContentPane();
        contenedor.setSize(AnchoVentana, AltoVentana);

        //agregar la ventana en el contenedor
        contenedor.add(this, BorderLayout.CENTER);

        //definir hilo
        hilo = new Thread();
    }

    @Override
    protected void paintComponent(Graphics grphcs) {
        super.paintComponent(grphcs);        
        for (int i = 0; i < this.FIGURA.length; i++) {
            //iterador para el ancho en bits de la figura
            for (int j = 0; j < this.ANCHO_BITS; j++) {
                int temp = this.FIGURA[i] & (this.MASCARA << j);
                if (temp != 0) {
                    grphcs.drawLine(
                            this.coordenada_y + j+10,
                            this.coordenada_x + i+10,
                            this.coordenada_y + j+10,
                            this.coordenada_x + i+10);
                }
            }
            grphcs.setColor(Color.BLUE);
        }
    }

    public void dibujar() {
        try {
            int contador = 0;
            int opc;
            while (contador < 10) {

                opc = (int) (Math.random() * 5);
                switch (opc) {
                    case 1:
                        ABAJO();
                        contador += 1;
                        System.out.println("¡OUCH! " + contador);
                        break;
                    case 2:
                        ARRIBA();
                        contador += 1;
                        System.out.println("¡OUCH! " + contador);
                        break;
                    case 3:
                        DERECHA();
                        contador += 1;
                        System.out.println("¡OUCH! " + contador);
                        break;
                    case 4:
                        IZQUIERDA();
                        contador += 1;
                        System.out.println("¡OUCH! " + contador);
                        break;
                    default:
                }
            }

        } catch (Exception e) {
            System.out.println("Error " + e);
        }

    }
    //Metodo que recorre hacia abajo
    public void ABAJO() {
        coordenada_x = (int) (Math.random() * 500);
        coordenada_y = (int) (Math.random() * 500);
        for (int i = coordenada_x + 60; i < AltoVentana; i++) {
            try {

                coordenada_x = coordenada_x + 1;
                hilo.sleep(tiempofinal);
                paint(getGraphics());

            } catch (Exception e) {
                System.out.println(e);
            }
            System.out.println("Se recorrieron = " + i + " hacia abajo");
        }        
    }

    //Metodo que recorre hacia arriba
    public void ARRIBA() {
        this.coordenada_x = (int) (Math.random() * 500);
        this.coordenada_y = (int) (Math.random() * 500);
        for (int i = coordenada_x; i > 0; i--) {
            try {

                coordenada_x = this.coordenada_x - 1;
                hilo.sleep(tiempofinal);
                paint(getGraphics());

            } catch (Exception e) {
                System.out.println(e);
            }
            System.out.println("Se recorrio = " + i + " hacia arriba");
        }
    }

    //Metodo que recorre a la derecha
    public void DERECHA() {
        coordenada_x = (int) (Math.random() * 500);
        coordenada_y = (int) (Math.random() * 500);
        for (int i = coordenada_y + 30; i < AnchoVentana; i++) {
            try {
                this.coordenada_y = coordenada_y + 1;
                this.hilo.sleep(tiempofinal);
                paint(getGraphics());

            } catch (Exception e) {
                System.out.println(e);
            }
            System.out.println("Se recorrio = " + i + " hacia la derecha");
        }

    }

    //Metodo que recorre a la izquierda
    public void IZQUIERDA() {
        coordenada_x = (int) (Math.random() * 500);
        coordenada_y = (int) (Math.random() * 500);
        for (int i = coordenada_y; i > 0; i--) {
            try {
                this.coordenada_y = coordenada_y - 1;
                this.hilo.sleep(tiempofinal);
                paint(getGraphics());

            } catch (Exception e) {
                System.out.println(e);
            }
            System.out.println("Se recorrio = " + i + " hacia la izquierda");
        }
    }

    public static void main(String[] args) {
        Animacion animacion = new Animacion();
        animacion.dibujar();
    }
}
