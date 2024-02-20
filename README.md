import java.util.Scanner;
import java.awt.Color;
import javax.swing.JFrame;
import javax.swing.JTable;
import javax.swing.table.DefaultTableCellRenderer;

public class Horario {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingrese el número de materias: ");
        int numberOfSubjects = scanner.nextInt();
        scanner.nextLine(); // Consumir el carácter de nueva línea

        String[] subjects = new String[numberOfSubjects];
        int[] numberOfClasses = new int[numberOfSubjects];
        String[][] days = new String[numberOfSubjects][];
        String[][] startTimes = new String[numberOfSubjects][];
        String[][] endTimes = new String[numberOfSubjects][];
        // Cambiar el tipo de dato de int a String y usar una coma en lugar de un punto y coma
        String[][] classroomNumbers = new String[numberOfSubjects][];

        // Eliminar la declaración del arreglo groupNumbers
        // int[][] groupNumbers = new int[numberOfSubjects][];

        for (int i = 0; i < numberOfSubjects; i++) {
            System.out.println("Ingrese los detalles de la materia " + (i + 1) + ":");

            System.out.print("Nombre de la materia: ");
            subjects[i] = scanner.nextLine();

            System.out.print("Número de clases por semana: ");
            numberOfClasses[i] = scanner.nextInt();
            scanner.nextLine(); // Consumir el carácter de nueva línea

            // Crear arreglos para almacenar los datos de cada clase
            days[i] = new String[numberOfClasses[i]];
            startTimes[i] = new String[numberOfClasses[i]];
            endTimes[i] = new String[numberOfClasses[i]];
            classroomNumbers[i] = new String[numberOfClasses[i]];

            // Pedir los datos de cada clase
            for (int j = 0; j < numberOfClasses[i]; j++) {
                System.out.println("Ingrese los detalles de la clase " + (j + 1) + ":");

                System.out.print("Día (por ejemplo, LU, MA): ");
                days[i][j] = scanner.nextLine();

                System.out.print("Hora de inicio (por ejemplo, 06:45): ");
                startTimes[i][j] = scanner.nextLine();

                System.out.print("Hora de fin (por ejemplo, 07:30): ");
                endTimes[i][j] = scanner.nextLine();

                // Eliminar la línea que pide el número de grupo
                // System.out.print("Número de grupo: ");
                // String groupNumber = scanner.nextLine();

                // Eliminar la línea que asigna el número de grupo
                // if (groupNumber.equals("-")) {
                //     groupNumbers[i][j] = -1;
                // } else {
                //     groupNumbers[i][j] = Integer.parseInt(groupNumber);
                // }

                // Cambiar el método scanner.nextInt() por scanner.nextLine()
                System.out.print("Número de aula: ");
                classroomNumbers[i][j] = scanner.nextLine();
                scanner.nextLine(); // Consumir el carácter de nueva línea
            }
        }

        // Crear un arreglo bidimensional para almacenar el horario
        String[][] schedule = new String[9][6];

        // Llenar la primera fila con los días de la semana
        schedule[0][0] = "";
        schedule[0][1] = "Lunes";
        schedule[0][2] = "Martes";
        schedule[0][3] = "Miércoles";
        schedule[0][4] = "Jueves";
        schedule[0][5] = "Viernes";

        // Llenar la primera columna con los horarios de las clases
        schedule[1][0] = "08:15 - 09:45";
        schedule[2][0] = "09:45 - 11:15";
        schedule[3][0] = "11:15 - 12:45";
        schedule[4][0] = "12:45 - 14:15";
        schedule[5][0] = "14:15 - 15:45";
        schedule[6][0] = "15:45 - 17:15";
        schedule[7][0] = "17:15 - 18:45";
        schedule[8][0] = "18:45 - 20:15";

        // Llenar el resto de la matriz con los datos de las materias
        for (int i = 0; i < numberOfSubjects; i++) {
            // Recorrer las clases de cada materia
            for (int j = 0; j < numberOfClasses[i]; j++) {
                // Verificar si el día, la hora o el aula son vacíos
                if (days[i][j].equals("-") || startTimes[i][j].equals("-") || endTimes[i][j].equals("-") || classroomNumbers[i][j].equals("-")) {
                    // Saltar la asignación de la celda
                    continue;
                }

                // Obtener el índice de la columna según el día de la clase
                int columnIndex = 0;
                switch (days[i][j]) {
                    case "LU":
                        columnIndex = 1;
                        break;
                    case "MA":
                        columnIndex = 2;
                        break;
                    case "MI":
                        columnIndex = 3;
                        break;
                    case "JU":
                        columnIndex = 4;
                        break;
                    case "VI":
                        columnIndex = 5;
                        break;
                }

                // Obtener el índice de la fila según la hora de inicio de la clase
                int rowIndex = 0;
                switch (startTimes[i][j]) {
                    case "08:15":
                        rowIndex = 1;
                        break;
                    case "09:45":
                        rowIndex = 2;
                        break;
                    case "11:15":
                        rowIndex = 3;
                        break;
                    case "12:45":
                        rowIndex = 4;
                        break;
                    case "14:15":
                        rowIndex = 5;
                        break;
                    case "15:45":
                        rowIndex = 6;
                        break;
                    case "17:15":
                        rowIndex = 7;
                        break;
                    case "18:45":
                        rowIndex = 8;
                        break;
                    case "20:15":
                        rowIndex = 9;
                        break;
                }

                // Llenar la celda correspondiente con el nombre de la materia y el número de aula
                // Usar el método String.format() para formatear el texto y el carácter <br> para hacer un salto de línea
                // Eliminar el número de grupo del formato
                schedule[rowIndex][columnIndex] = String.format("%s<br>A%s", subjects[i], classroomNumbers[i][j]);
            }
        }

        // Crear una ventana para mostrar el horario
        JFrame frame = new JFrame("Horario");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(800, 600);

        // Crear una tabla para mostrar el horario
        JTable table = new JTable(schedule, schedule[0]);
        table.setRowHeight(40);
        table.setAutoResizeMode(JTable.AUTO_RESIZE_ALL_COLUMNS);

        // Crear un renderizador para centrar el texto de las celdas
        DefaultTableCellRenderer renderer = new DefaultTableCellRenderer();
        renderer.setHorizontalAlignment(DefaultTableCellRenderer.CENTER);

        // Asignar el renderizador a todas las columnas
        for (int i = 0; i < 7; i++) {
            table.getColumnModel().getColumn(i).setCellRenderer(renderer);
        }

        // Agregar la tabla a la ventana
        frame.add(table);
        frame.setVisible(true);
    }
}
