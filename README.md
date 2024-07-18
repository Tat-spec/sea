# seaimport numpy as np
import matplotlib.pyplot as plt

class SeaweedSimulator:
    def __init__(self, size=(50, 50), initial_seaweed_density=0.1, growth_rate=0.1):
        self.size = size  # Size of the 2D grid (ocean)
        self.grid = np.zeros(size)  # Initialize the ocean grid with zeros
        self.growth_rate = growth_rate  # Growth rate of seaweed
        self.initialize_seaweed(initial_seaweed_density)  # Initialize seaweed density
    
    def initialize_seaweed(self, initial_density):
        """Randomly initialize seaweed on the grid."""
        num_seaweed = int(initial_density * np.prod(self.size))
        positions = np.random.choice(np.prod(self.size), size=num_seaweed, replace=False)
        self.grid = np.zeros(self.size)
        self.grid.flat[positions] = 1
    
    def simulate_growth(self, num_steps):
        """Simulate seaweed growth for a number of time steps."""
        for _ in range(num_steps):
            new_grid = np.zeros(self.size)
            for i in range(self.size[0]):
                for j in range(self.size[1]):
                    if self.grid[i, j] == 1:
                        # Seaweed grows in all adjacent cells (8 directions)
                        for di in [-1, 0, 1]:
                            for dj in [-1, 0, 1]:
                                ni, nj = i + di, j + dj
                                if 0 <= ni < self.size[0] and 0 <= nj < self.size[1]:
                                    new_grid[ni, nj] += self.growth_rate
            self.grid += new_grid  # Update the seaweed grid
    
    def plot_seaweed(self):
        """Plot the current state of the seaweed grid."""
        plt.imshow(self.grid, cmap='Greens', interpolation='nearest')
        plt.colorbar(label='Seaweed density')
        plt.title('Seaweed Growth Simulation')
        plt.xlabel('X')
        plt.ylabel('Y')
        plt.show()

# Example usage:
if __name__ == "__main__":
    seaweed_sim = SeaweedSimulator(size=(50, 50), initial_seaweed_density=0.05, growth_rate=0.05)
    seaweed_sim.simulate_growth(num_steps=100)
    seaweed_sim.plot_seaweed()
