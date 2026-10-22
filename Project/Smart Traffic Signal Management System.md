#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define INF 9999
#define MAX 100
#define ROADS 4

struct Vehicle
{
    char number[20];
    char type[20];
    int road;
    int emergency;
};

struct Vehicle vehicles[MAX];
int count = 0;

struct Vehicle queue[ROADS][MAX];
int front[ROADS] = {0};
int rear[ROADS] = {0};

/* MODULE 1: VEHICLE ENTRY */
void addVehicle()
{
    struct Vehicle v;
    int i;

    if (count >= MAX)
    {
        printf("Vehicle storage is full!\n");
        return;
    }

    printf("\nEnter vehicle number: ");
    scanf("%19s", v.number);

    if (strlen(v.number) > 10)
    {
        printf("Vehicle number must not exceed 10 characters!\n");
        return;
    }

    for (i = 0; i < count; i++)
    {
        if (strcmp(vehicles[i].number, v.number) == 0)
        {
            printf("Duplicate vehicle number! Entry rejected.\n");
            return;
        }
    }

    printf("Enter vehicle type (Car/Bike/Bus/Ambulance/FireTruck): ");
    scanf("%19s", v.type);

    printf("Enter road number (1-4): ");
    if (scanf("%d", &v.road) != 1)
    {
        printf("Invalid road number!\n");
        return;
    }

    if (v.road < 1 || v.road > ROADS)
    {
        printf("Invalid road number!\n");
        return;
    }

    if (rear[v.road - 1] >= MAX)
    {
        printf("Road queue is full!\n");
        return;
    }

    if (strcmp(v.type, "Ambulance") == 0 ||
        strcmp(v.type, "FireTruck") == 0)
    {
        v.emergency = 1;
    }
    else
    {
        v.emergency = 0;
    }

    vehicles[count] = v;
    count++;

    queue[v.road - 1][rear[v.road - 1]] = v;
    rear[v.road - 1]++;

    printf("Vehicle added successfully!\n");

    if (v.emergency)
        printf("Emergency vehicle detected. Priority given.\n");
}

/* MODULE 2: DISPLAY TRAFFIC QUEUES */
void displayQueues()
{
    int i, j;

    printf("\n========== TRAFFIC QUEUES ==========\n");

    for (i = 0; i < ROADS; i++)
    {
        printf("\nRoad %d:\n", i + 1);

        if (front[i] == rear[i])
        {
            printf("No vehicles\n");
        }
        else
        {
            for (j = front[i]; j < rear[i]; j++)
            {
                printf("%s - %s",
                       queue[i][j].number,
                       queue[i][j].type);

                if (queue[i][j].emergency)
                    printf(" [EMERGENCY]");

                printf("\n");
            }
        }
    }
}

/* MODULE 3: DEQUEUE WITH EMERGENCY PRIORITY */
void dequeueVehicle()
{
    int road, i, j, index = -1;

    printf("\nEnter road number: ");

    if (scanf("%d", &road) != 1)
    {
        printf("Invalid road number!\n");
        return;
    }

    if (road < 1 || road > ROADS)
    {
        printf("Invalid road number!\n");
        return;
    }

    road--;

    if (front[road] == rear[road])
    {
        printf("Road %d queue is empty.\n", road + 1);
        return;
    }

    /* Find the first waiting emergency vehicle */
    for (i = front[road]; i < rear[road]; i++)
    {
        if (queue[road][i].emergency)
        {
            index = i;
            break;
        }
    }

    /* If there is no emergency, serve the first vehicle */
    if (index == -1)
        index = front[road];

    if (queue[road][index].emergency)
    {
        printf("Emergency vehicle %s passed first from Road %d.\n",
               queue[road][index].number, road + 1);
    }
    else
    {
        printf("Vehicle %s has passed from Road %d.\n",
               queue[road][index].number, road + 1);
    }

    /* Remove the selected vehicle and shift remaining vehicles */
    for (j = index; j < rear[road] - 1; j++)
    {
        queue[road][j] = queue[road][j + 1];
    }

    rear[road]--;
}

/* MODULE 4: TRAFFIC DENSITY ANALYSIS */
void densityAnalysis()
{
    int i, vehiclesOnRoad;

    printf("\n========== TRAFFIC DENSITY ==========\n");

    for (i = 0; i < ROADS; i++)
    {
        vehiclesOnRoad = rear[i] - front[i];

        printf("Road %d: %d vehicles - ",
               i + 1, vehiclesOnRoad);

        if (vehiclesOnRoad <= 2)
            printf("LOW\n");
        else if (vehiclesOnRoad <= 5)
            printf("MEDIUM\n");
        else
            printf("HIGH\n");
    }
}

/* MODULE 5: SMART SIGNAL MANAGEMENT */
void signalManagement()
{
    int i, j, temp;
    int road[ROADS] = {0, 1, 2, 3};
    int density[ROADS];
    int emergency[ROADS];

    for (i = 0; i < ROADS; i++)
    {
        density[i] = rear[i] - front[i];
        emergency[i] = 0;

        for (j = front[i]; j < rear[i]; j++)
        {
            if (queue[i][j].emergency)
            {
                emergency[i] = 1;
                break;
            }
        }
    }

    /* Emergency roads first, then highest traffic density */
    for (i = 0; i < ROADS - 1; i++)
    {
        for (j = i + 1; j < ROADS; j++)
        {
            if (emergency[road[j]] > emergency[road[i]] ||
                (emergency[road[j]] == emergency[road[i]] &&
                 density[road[j]] > density[road[i]]))
            {
                temp = road[i];
                road[i] = road[j];
                road[j] = temp;
            }
        }
    }

    printf("\n========== SMART SIGNAL ==========\n");

    for (i = 0; i < ROADS; i++)
    {
        int r = road[i];
        int time;

        if (density[r] <= 2)
            time = 20;
        else if (density[r] <= 5)
            time = 40;
        else
            time = 60;

        printf("\n%d. Road %d", i + 1, r + 1);

        if (emergency[r])
            printf(" -> EMERGENCY PRIORITY");

        printf("\n   Vehicles: %d", density[r]);
        printf("\n   Green Light: %d seconds\n", time);
    }
}

/* MODULE 6: EMERGENCY VEHICLE PRIORITY */
void emergencyPriority()
{
    int i, j, found = 0;

    printf("\n========== EMERGENCY PRIORITY ==========\n");

    for (i = 0; i < ROADS; i++)
    {
        for (j = front[i]; j < rear[i]; j++)
        {
            if (queue[i][j].emergency)
            {
                found = 1;

                printf("Emergency vehicle %s detected on Road %d\n",
                       queue[i][j].number, i + 1);

                printf("Road %d gets PRIORITY.\n", i + 1);
            }
        }
    }

    if (!found)
        printf("No emergency vehicles currently waiting.\n");
}

/* MODULE 7: TRAFFIC REPORT */
void trafficReport()
{
    int i, j;
    int highestRoad = 0;
    int highestCount = 0;
    int emergencyCount = 0;

    printf("\n========== TRAFFIC REPORT ==========\n");
    printf("Total vehicles entered: %d\n", count);

    for (i = 0; i < ROADS; i++)
    {
        int roadCount = rear[i] - front[i];

        printf("Vehicles on Road %d: %d\n",
               i + 1, roadCount);

        if (roadCount > highestCount)
        {
            highestCount = roadCount;
            highestRoad = i + 1;
        }

        for (j = front[i]; j < rear[i]; j++)
        {
            if (queue[i][j].emergency)
                emergencyCount++;
        }
    }

    if (highestCount > 0)
        printf("Highest-density road: Road %d\n", highestRoad);
    else
        printf("Highest-density road: None\n");

    printf("Emergency vehicles waiting: %d\n", emergencyCount);

    printf("\nSignal Timings:\n");

    for (i = 0; i < ROADS; i++)
    {
        int roadCount = rear[i] - front[i];
        int time;

        if (roadCount <= 2)
            time = 20;
        else if (roadCount <= 5)
            time = 40;
        else
            time = 60;

        printf("Road %d: %d seconds\n", i + 1, time);
    }
}

/* MODULE 8: SHORTEST PATH USING DIJKSTRA'S ALGORITHM */
void dijkstra()
{
    int graph[4][4] = {
        {0, 5, 2, 0},
        {5, 0, 0, 3},
        {2, 0, 0, 4},
        {0, 3, 4, 0}
    };

    int distance[4];
    int visited[4] = {0};
    int parent[4];
    int source, destination;
    int i, j, min, next;

    printf("\nEnter source junction (1-4): ");

    if (scanf("%d", &source) != 1)
    {
        printf("Invalid source junction!\n");
        return;
    }

    printf("Enter destination junction (1-4): ");

    if (scanf("%d", &destination) != 1)
    {
        printf("Invalid destination junction!\n");
        return;
    }

    if (source < 1 || source > 4 ||
        destination < 1 || destination > 4)
    {
        printf("Invalid junction number!\n");
        return;
    }

    source--;
    destination--;

    for (i = 0; i < 4; i++)
    {
        distance[i] = INF;
        parent[i] = -1;
    }

    distance[source] = 0;

    for (i = 0; i < 4; i++)
    {
        min = INF;
        next = -1;

        for (j = 0; j < 4; j++)
        {
            if (!visited[j] && distance[j] < min)
            {
                min = distance[j];
                next = j;
            }
        }

        if (next == -1)
            break;

        visited[next] = 1;

        for (j = 0; j < 4; j++)
        {
            if (graph[next][j] != 0 &&
                !visited[j] &&
                distance[next] + graph[next][j] < distance[j])
            {
                distance[j] = distance[next] + graph[next][j];
                parent[j] = next;
            }
        }
    }

    if (distance[destination] == INF)
    {
        printf("No route available.\n");
        return;
    }

    printf("\nShortest distance: %d units\n",
           distance[destination]);

    printf("Shortest Path: ");

    int path[4];
    int length = 0;
    int current = destination;

    while (current != -1)
    {
        path[length++] = current;
        current = parent[current];
    }

    for (i = length - 1; i >= 0; i--)
    {
        printf("%d", path[i] + 1);

        if (i != 0)
            printf(" -> ");
    }

    printf("\n");
}

/* DISPLAY ALL VEHICLES ENTERED */
void displayVehicles()
{
    int i;

    if (count == 0)
    {
        printf("\nNo vehicles entered.\n");
        return;
    }

    printf("\n========== VEHICLE DETAILS ==========\n");

    for (i = 0; i < count; i++)
    {
        printf("\nVehicle %d\n", i + 1);
        printf("Number : %s\n", vehicles[i].number);
        printf("Type   : %s\n", vehicles[i].type);
        printf("Road   : %d\n", vehicles[i].road);

        if (vehicles[i].emergency)
            printf("Priority: EMERGENCY\n");
        else
            printf("Priority: NORMAL\n");
    }
}

/* MAIN FUNCTION */
int main()
{
    int choice;

    do
    {
        printf("\n========================================\n");
        printf("     SMART TRAFFIC MANAGEMENT SYSTEM\n");
        printf("========================================\n");

        printf("1. Vehicle Entry\n");
        printf("2. Display Vehicles\n");
        printf("3. Display Traffic Queues\n");
        printf("4. Dequeue Vehicle\n");
        printf("5. Traffic Density Analysis\n");
        printf("6. Smart Signal Management\n");
        printf("7. Emergency Vehicle Priority\n");
        printf("8. Traffic Report\n");
        printf("9. Shortest Path\n");
        printf("10. Exit\n");

        printf("\nEnter your choice: ");

        if (scanf("%d", &choice) != 1)
        {
            printf("Invalid input!\n");
            break;
        }

        switch (choice)
        {
            case 1:
                addVehicle();
                break;

            case 2:
                displayVehicles();
                break;

            case 3:
                displayQueues();
                break;

            case 4:
                dequeueVehicle();
                break;

            case 5:
                densityAnalysis();
                break;

            case 6:
                signalManagement();
                break;

            case 7:
                emergencyPriority();
                break;

            case 8:
                trafficReport();
                break;

            case 9:
                dijkstra();
                break;

            case 10:
                printf("Exiting Smart Traffic Management System.\n");
                break;

            default:
                printf("\nInvalid choice!\n");
        }

    } while (choice != 10);

    return 0;
} 

