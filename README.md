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
# CODE FOR FIRST DIAGRAM ENDS HERE



 #  CODE FOR SECOND DIAGRAM STARTS HERE
 import matplotlib.pyplot as plt
import numpy as np

# Gamma-ray attenuation in lead
x_lead = np.linspace(0, 10, 100)  # Thickness of lead (cm) from 0 to 10 cm
mu_lead = 0.5  # Linear attenuation coefficient for lead (cm^-1)
I_gamma_lead = 100 * np.exp(-mu_lead * x_lead)  # Transmitted gamma intensity (%)

# Neutron attenuation in HDPE
x_hdpe = np.linspace(0, 20, 100)  # Thickness of HDPE (cm) from 0 to 20 cm
sigma_hdpe = 0.1  # Removal cross-section for HDPE (cm^-1)
I_neutron_hdpe = 100 * np.exp(-sigma_hdpe * x_hdpe)  # Transmitted neutron intensity (%)

# Neutron attenuation in borated polyethylene
x_borated = np.linspace(0, 10, 100)  # Thickness of borated polyethylene (cm) from 0 to 10 cm
sigma_borated = 0.3  # Assumed removal cross-section for borated polyethylene (cm^-1)
I_neutron_borated = 100 * np.exp(-sigma_borated * x_borated)  # Transmitted neutron intensity (%)

# Gamma-ray attenuation in stainless steel
x_steel = np.linspace(0, 5, 100)  # Thickness of stainless steel (cm) from 0 to 5 cm
mu_steel = 0.2  # Assumed linear attenuation coefficient for stainless steel (cm^-1)
I_gamma_steel = 100 * np.exp(-mu_steel * x_steel)  # Transmitted gamma intensity (%)

# Plotting the graph
plt.figure(figsize=(12, 8))

# Gamma-ray attenuation curves
plt.plot(x_lead, I_gamma_lead, label='Gamma-Ray Attenuation in Lead', color='blue', linewidth=2)
plt.plot(x_steel, I_gamma_steel, label='Gamma-Ray Attenuation in Stainless Steel', color='green', linewidth=2)

# Neutron attenuation curves
plt.plot(x_hdpe, I_neutron_hdpe, label='Neutron Attenuation in HDPE', color='red', linewidth=2)
plt.plot(x_borated, I_neutron_borated, label='Neutron Attenuation in Borated Polyethylene', color='purple', linewidth=2)

# Highlighting specific points from the report
# Gamma-ray attenuation at 7 cm of lead
plt.scatter(7, 100 * np.exp(-0.5 * 7), color='blue', zorder=5)
plt.text(7, 100 * np.exp(-0.5 * 7) + 2, f'7 cm: {100 * np.exp(-0.5 * 7):.1f}%', fontsize=10, color='blue')

# Neutron attenuation at 15 cm of HDPE
plt.scatter(15, 100 * np.exp(-0.1 * 15), color='red', zorder=5)
plt.text(15, 100 * np.exp(-0.1 * 15) + 2, f'15 cm: {100 * np.exp(-0.1 * 15):.1f}%', fontsize=10, color='red')

# Neutron attenuation at 5 cm of borated polyethylene
plt.scatter(5, 100 * np.exp(-0.3 * 5), color='purple', zorder=5)
plt.text(5, 100 * np.exp(-0.3 * 5) + 2, f'5 cm: {100 * np.exp(-0.3 * 5):.1f}%', fontsize=10, color='purple')

# Gamma-ray attenuation at 2 cm of stainless steel
plt.scatter(2, 100 * np.exp(-0.2 * 2), color='green', zorder=5)
plt.text(2, 100 * np.exp(-0.2 * 2) + 2, f'2 cm: {100 * np.exp(-0.2 * 2):.1f}%', fontsize=10, color='green')

# Adding labels, title, and grid
plt.xlabel('Thickness (cm)', fontsize=12)
plt.ylabel('Transmitted Intensity (%)', fontsize=12)
plt.title('Radiation Attenuation in Shielding Materials', fontsize=14)
plt.legend(fontsize=12)
plt.grid(True, linestyle='--', alpha=0.6)

# Display the graph
plt.show()
