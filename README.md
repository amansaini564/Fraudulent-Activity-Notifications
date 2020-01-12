# Fraudulent-Activity-Notifications
Solution to Hackerrank Fraudulent Notifications Problem in Java
(Its for Educational Purpose Only and not for any type of Infringement)

    import java.io.*;
    import java.math.*;
    import java.security.*;
    import java.text.*;
    import java.util.*;
    import java.util.concurrent.*;
    import java.util.regex.*;

    public class Solution {
        // Complete the activityNotifications function below.
        static int activityNotifications(int[] expenditure, int d) {
            int result=0;
            int[] countArray = new int[201];

            int lengExp = expenditure.length;

            Arrays.fill(countArray, 0);

            int medTerm = d/2 + 1;

            boolean flag = false;
            if(d%2 == 0){
                flag = true;
                medTerm--;
            }

            for(int j=0; j<d; ++j){
                countArray[expenditure[j]]++;
            }

            int count = 0;
            float medValue = 0;
            for(int i=0; i<lengExp-d; ++i){
                for(int j=0; j<201; ++j){
                    count += countArray[j];
                    if(flag == false){
                        if(count >= medTerm){
                            medValue = j;
                            break;
                        }
                    }
                    else{
                        if(count == medTerm){
                            medValue = j;
                            int k = j+1;
                            while(countArray[k] == 0){
                                ++k;
                            }
                            medValue += k;
                            medValue /= 2;
                            break;
                        }
                        if(count > medTerm){
                            medValue = j;
                            break;
                        }
                    }
                }

                if( expenditure[i+d] >= 2*medValue ){
                    ++result;
                }         

                countArray[expenditure[i]]--;
                countArray[expenditure[i+d]]++;

                count  = 0;
                medValue = 0;
            }
            return result;
        }

        private static final Scanner scanner = new Scanner(System.in);

        public static void main(String[] args) throws IOException {
            BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(System.getenv("OUTPUT_PATH")));

            String[] nd = scanner.nextLine().split(" ");

            int n = Integer.parseInt(nd[0]);

            int d = Integer.parseInt(nd[1]);

            int[] expenditure = new int[n];

            String[] expenditureItems = scanner.nextLine().split(" ");
            scanner.skip("(\r\n|[\n\r\u2028\u2029\u0085])?");

            for (int i = 0; i < n; i++) {
                int expenditureItem = Integer.parseInt(expenditureItems[i]);
                expenditure[i] = expenditureItem;
            }

            int result = activityNotifications(expenditure, d);

            bufferedWriter.write(String.valueOf(result));
            bufferedWriter.newLine();

            bufferedWriter.close();

            scanner.close();
        }
    }
