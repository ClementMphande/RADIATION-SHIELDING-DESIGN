# RADIATION-SHIELDING-DESIGN
THIS IS A CODE USED TO DESIGN SHIELDING RADIATION WITH PYTHON AND GOOGLE COLAB
import matplotlib.pyplot as plt
import numpy as np

def draw_shielding():
    fig, ax = plt.subplots(figsize=(10, 8))  # Slightly wider figure to accommodate the table
    ax.set_xlim(-40, 40)
    ax.set_ylim(-40, 40)
    
    # Define accurate layer radii based on real thickness
    hdpe_radius = 15  # Core (15 cm)
    polyethylene_radius = hdpe_radius + 5  # Borated Polyethylene (5 cm)
    steel_radius = polyethylene_radius + 2  # Stainless Steel (2 cm)
    lead_radius = steel_radius + 7  # Lead (7 cm outermost)
    
    layers = [lead_radius, steel_radius, polyethylene_radius, hdpe_radius]
    colors = ['black', 'gray', 'green', 'blue']  # Layer colors
    labels = ['Lead (7 cm)', 'Stainless Steel (2 cm)', 'Borated Polyethylene (5 cm)', 'HDPE (15 cm)']
    
    # Draw concentric circles with proper spacing
    for i, radius in enumerate(layers):
        circle = plt.Circle((0, 0), radius, color=colors[i], alpha=0.6, ec='black')
        ax.add_patch(circle)
    
    # Add arrows for attenuation with proper spacing
    ax.arrow(0, 0, 0, 25, head_width=2, head_length=3, fc='red', ec='red', alpha=0.8)  # Gamma attenuation
    ax.arrow(0, 0, -15, 13, head_width=2, head_length=3, fc='blue', ec='blue', alpha=0.8, linestyle='dashed')  # Neutron attenuation (stops at y=13)
    
    # Display attenuation percentages clearly without overlapping arrows
    ax.text(5, 27, '97% Gamma Attenuation', color='red', fontsize=10, bbox=dict(facecolor='white', alpha=0.8), ha='left')  # Adjust x, y for positioning
    ax.text(-20, 15, '77.7% Neutron Attenuation', color='blue', fontsize=10, bbox=dict(facecolor='white', alpha=0.8), ha='left')  # Adjust x, y for positioning
    
    # Add legend at the top
    legend_elements = [
        plt.Line2D([0], [0], color='blue', lw=4, label='HDPE (Neutron Moderation)'),
        plt.Line2D([0], [0], color='green', lw=4, label='Borated Polyethylene (Thermal Neutron Absorption)'),
        plt.Line2D([0], [0], color='gray', lw=4, label='Stainless Steel (Structural Support)'),
        plt.Line2D([0], [0], color='black', lw=4, label='Lead (Gamma Attenuation)'),
        plt.Line2D([0], [0], color='red', lw=2, label='Gamma Rays'),
        plt.Line2D([0], [0], color='blue', linestyle='dashed', lw=2, label='Neutrons')
    ]
    ax.legend(handles=legend_elements, loc='upper center', fontsize=10, framealpha=0.5, bbox_to_anchor=(0.5, 1.1), ncol=3)  # Legend at the top
    
    # Add labels at the bottom horizontally
    label_positions = [-30, -10, 10, 30]  # Adjust x-positions for labels (more spaced out)
    for i, label in enumerate(labels):
        ax.text(label_positions[i], -35, label, fontsize=12, color='white', bbox=dict(facecolor=colors[i], alpha=0.5), ha='center')  # Adjust y for vertical positioning
    
    ax.set_xticks([])
    ax.set_yticks([])
    ax.set_frame_on(False)
    
    plt.tight_layout()  # Ensure no overlapping elements
    plt.show()

draw_shielding()
