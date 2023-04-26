# Wavelabs-Project
#include <iostream>
#include <vector>

using namespace std;

vector<vector<int>> adj_list;
vector<int> discovery_time;
vector<int> low_time;
vector<vector<int>> result;

void dfs(int node, int parent, int& time) {
    discovery_time[node] = low_time[node] = ++time;

    for (int neighbor : adj_list[node]) {
        if (neighbor == parent) {
            continue;  // skip the parent node
        }

        if (discovery_time[neighbor] == 0) {
            dfs(neighbor, node, time);
            low_time[node] = min(low_time[node], low_time[neighbor]);

            if (low_time[neighbor] > discovery_time[node]) {
                result.push_back({node, neighbor});
            }
        } else {
            low_time[node] = min(low_time[node], discovery_time[neighbor]);
        }
    }
}

vector<vector<int>> find_critical_connections(int n, vector<vector<int>>& connections) {
    adj_list.resize(n);
    discovery_time.resize(n);
    low_time.resize(n);

    for (auto& connection : connections) {
        int u = connection[0];
        int v = connection[1];
        adj_list[u].push_back(v);
        adj_list[v].push_back(u);
    }

    int time = 0;
    dfs(0, -1, time);  // start DFS from node 0

    return result;
}

int main() {
    int n = 4;
    vector<vector<int>> connections = {{0,1},{1,2},{2,0},{1,3}};

    vector<vector<int>> result = find_critical_connections(n, connections);

    cout << "Critical connections:" << endl;
    for (auto& connection : result) {
        cout << connection[0] << " " << connection[1] << endl;
    }

    return 0;
}
