import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.geom.RoundRectangle2D;
import java.util.Random;

public class LotoFacil {

    private static char letraPremiada = 'E';
    private static Random random = new Random();

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> createAndShowGUI());
    }

    private static void createAndShowGUI() {
        JFrame frame = new JFrame("LOTOFÁCIL");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));
        panel.setPreferredSize(new Dimension(300, 240));

        JLabel label = new JLabel("Menu LOTOFÁCIL:");
        label.setAlignmentX(Component.CENTER_ALIGNMENT);
        panel.add(label);

        JButton button1 = new RoundButton("Faça uma aposta de 0 a 100.");
        button1.addActionListener(e -> apostarDe0a100());
        button1.setAlignmentX(Component.CENTER_ALIGNMENT);
        panel.add(button1);

        panel.add(Box.createRigidArea(new Dimension(0, 10)));

        JButton button2 = new RoundButton("Faça uma aposta de A à Z.");
        button2.addActionListener(e -> apostarDeAaZ());
        button2.setAlignmentX(Component.CENTER_ALIGNMENT);
        panel.add(button2);

        panel.add(Box.createRigidArea(new Dimension(0, 10)));

        JButton button3 = new RoundButton("Selecione um número par ou ímpar.");
        button3.addActionListener(e -> apostarParOuImpar());
        button3.setAlignmentX(Component.CENTER_ALIGNMENT);
        panel.add(button3);

        panel.add(Box.createRigidArea(new Dimension(0, 10)));

        JButton buttonExit = new RoundButton("Sair");
        buttonExit.addActionListener(e -> System.exit(0));
        buttonExit.setAlignmentX(Component.CENTER_ALIGNMENT);
        panel.add(buttonExit);

        frame.getContentPane().add(panel);
        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setVisible(true);
    }

    private static void apostarDe0a100() {
        int numeroEscolhido = Integer.parseInt(JOptionPane.showInputDialog("Digite um número de 0 a 100: "));
        if (numeroEscolhido < 0 || numeroEscolhido > 100) {
            JOptionPane.showMessageDialog(null, "Aposta inválida.");
        } else {
            int numeroSorteado = random.nextInt(101);
            if (numeroEscolhido == numeroSorteado) {
                JOptionPane.showMessageDialog(null, "Você ganhou R$ 1.000,00 reais.");
            } else {
                JOptionPane.showMessageDialog(null, "Que pena! O número sorteado foi: " + numeroSorteado);
            }
        }
    }

    private static void apostarDeAaZ() {
        String letraInput = JOptionPane.showInputDialog("Digite uma letra de A à Z: ");
        char letraEscolhida = letraInput.toUpperCase().charAt(0);
        if (Character.isLetter(letraEscolhida)) {
            if (letraEscolhida == letraPremiada) {
                JOptionPane.showMessageDialog(null, "Você ganhou R$ 500,00 reais.");
            } else {
                JOptionPane.showMessageDialog(null, "Que pena! A letra sorteada foi: " + letraPremiada);
            }
        } else {
            JOptionPane.showMessageDialog(null, "Aposta inválida.");
        }
    }

    private static void apostarParOuImpar() {
        int numeroDigitado = Integer.parseInt(JOptionPane.showInputDialog("Digite um número inteiro: "));
        if (numeroDigitado % 2 == 0) {
            JOptionPane.showMessageDialog(null, "Você ganhou R$ 100,00 reais.");
        } else {
            JOptionPane.showMessageDialog(null, "Que pena! O número digitado é ímpar e a premiação foi para números pares.");
        }
    }

    static class RoundButton extends JButton {
        public RoundButton(String label) {
            super(label);
            setRolloverEnabled(true); 
            setBackground(Color.white);
            setFocusPainted(false);
            setBorderPainted(false);
            setOpaque(true);
            setPreferredSize(new Dimension(200, 50));
            addMouseListener(new MouseAdapter() {
                @Override
                public void mouseEntered(MouseEvent e) {
                    setBackground(Color.lightGray);
                }

                @Override
                public void mouseExited(MouseEvent e) {
                    setBackground(Color.white);
                }
            });
        }

        protected void paintComponent(Graphics g) {
            if (getModel().isArmed()) {
                g.setColor(Color.white);
            } else {
                g.setColor(getBackground());
            }
            g.fillRoundRect(0, 0, getSize().width - 1, getSize().height - 1, 20, 20);

            super.paintComponent(g);
        }

        protected void paintBorder(Graphics g) {
            g.setColor(Color.darkGray);
            g.drawRoundRect(0, 0, getSize().width - 1, getSize().height - 1, 20, 20);
        }

        Shape shape;

        public boolean contains(int x, int y) {
            if (shape == null || !shape.getBounds().equals(getBounds())) {
                shape = new RoundRectangle2D.Float(0, 0, getWidth() - 1, getHeight() - 1, 20, 20);
            }
            return shape.contains(x, y);
        }
    }
}
