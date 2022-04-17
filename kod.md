import networkx as nx
import matplotlib.pyplot as plt
import random
import ast
​
class MyClass():
    def _init_(self):
        self.list2 = ast.literal_eval(input('Please Enter the values: '))
        self.list1 = []
        for a, b in self.list2:
            self.list1.append(a and b)
        for i in list(range(1,10)):
            for a in self.list1:
                while self.list1.count(a) > 1:
                    self.list1.remove(a)
        self.list1.sort()
        self.result = {}
        for first, second in self.list2:
            self.result.setdefault(first, []).append(second)
        self.reflexive_list = []
        for a, b in list(self.list2):
            if a == b:
                self.reflexive_list.append((a,b))
        self.primes = []
        for num in self.list1:
            prime = True
            for i in range(2,num):
                if (num%i==0):
                    prime = False
            if prime:
               self.primes.append(num)
            
    def check_reflexive(self):
        if len(self.reflexive_list) == len(self.list1):
            print(self.reflexive_list)
            print("Reflexive check ACCEPTED")
            return True
        else:
            print("Reflexive check NOT ACCEPTED")
            return False
        
    def check_antisymmetric(self):
        antisymettric_list = []
        for b in self.list2:
            swap1 = b[0]
            swap2 = b[1]
            newtuple = (swap2, swap1)
            antisymettric_list.append(newtuple)
        for ü in self.reflexive_list:
            if ü in antisymettric_list:
                antisymettric_list.remove(ü)
            else:
                None
        print(antisymettric_list)
        for q in antisymettric_list:
            if q in self.list2:
                print("Anti-Symmetric check NOT ACCEPTED")
                return False
        print("Anti-Symmetric check ACCEPTED")
        return True
    
    def check_transitive(self):      
        for a, b in self.list2:
            for x in self.result[b]:
                if (   x in self.result[a]  ):
                    None
                else:
                    print("Transitive check NOT ACCEPTED")
                    print("There is no ({},{}) in the {}".format(a, x, self.result[a]))
                    return False
        print("Transitive check ACCEPTED")
        return True
    
    def draw_diagram(self):
        pos = {}
        #origin = -len(list(self.result.keys()))-5
        randlist = list(range(1,len(list(self.result.keys()))+1))
        for a in (list(self.result.keys())):
            if a == 1:
                pos.setdefault(a, ((len(list(self.result.keys()))/2), -len(list(self.result.keys()))*2-4))
            elif a in self.primes:
                pos.setdefault(a, (a, -len(list(self.result.keys()))*2))
            elif len(list(self.result[a])) == 1:
                exitr = random.choice(randlist)
                pos.setdefault(a,(exitr, 0))
                randlist.remove(exitr)
            else:
                exitr = random.choice(randlist)
                pos.setdefault(a, (exitr, (-len(list(self.result[a])))*2))
                randlist.remove(exitr)
        ###
        edges = {}
        list1_reverse = list(self.list1)
        for a in list1_reverse:
            for b in list1_reverse:
                if (a%b==0) and (a != b):
                    edges.setdefault(a, []).append(b)
        ###
        edge_list = [(x,y) for x,y in self.list2 if x!=y]
        for i in list(range(1,10)):
            for a, b in edge_list:
                if b in list(edges.keys()):
                    for z in edges[b]:
                        if (z!=a) and (z%a==0):
                            while (a,b) in edge_list:
                                edge_list.remove((a,b))
        ###
        T = nx.DiGraph()
        T.add_nodes_from(list(pos.keys()))
        T.add_edges_from(edge_list)
        plt.figure()
        if ( (self.check_reflexive() == True) and (self.check_antisymmetric() == True) and (self.check_transitive()== True) ):
            nx.draw(T, pos, node_color='orange', node_size=600, font_size= 15, font_color='purple', with_labels=True, arrowsize=18, edge_color='green')
        else:
            return "There are conditions that are not provided."
