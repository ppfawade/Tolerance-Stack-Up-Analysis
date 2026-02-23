# Tolerance Stack-Up Analysis Tool

Advanced tolerance analysis tool with Monte Carlo simulation, RSS analysis, and clearance fit evaluation.

## 📚 What is Tolerance Stack-Up? (For Beginners)

### Understanding Tolerances

**Tolerance** is the permissible variation in a dimension. No manufacturing process is perfect—every part will have slight variations. For example:
- A shaft specified as 10.00 mm ±0.05 mm can be anywhere from 9.95 mm to 10.05 mm
- The ±0.05 mm is the **tolerance**
- 10.00 mm is the **nominal** (target) dimension

### Why Stack-Up Analysis Matters

When you assemble multiple parts, their individual tolerances **accumulate** or "stack up." This can cause:
- ✅ **Good outcome**: Parts fit together perfectly
- ❌ **Bad outcome**: Assembly won't fit, has excessive gaps, or doesn't function

**Real-world example**: A phone case assembly
- Battery compartment depth: 8.0 mm ±0.1 mm
- Battery thickness: 7.5 mm ±0.1 mm
- Battery cover thickness: 0.4 mm ±0.05 mm

Question: Will the battery fit? How much clearance will there be?

**Worst case**: 
- Smallest compartment: 8.0 - 0.1 = 7.9 mm
- Largest battery + cover: 7.5 + 0.1 + 0.4 + 0.05 = 8.05 mm
- **Problem!** 8.05 > 7.9 → Won't fit!

This is why tolerance stack-up analysis is critical in design.

---

## 🎓 Key Concepts Explained

### 1. **Bilateral vs Unilateral Tolerances**

**Bilateral Tolerance** (±):
- Variation goes both directions from nominal
- Example: 10 mm ±0.05 mm means 9.95 to 10.05 mm
- Most common in mechanical design

**Unilateral Tolerance** (+/- or -/0):
- Variation in only one direction
- Example: 10 mm +0.05/-0 means 10.00 to 10.05 mm
- Common for holes (always larger, never smaller)

### 2. **Analysis Methods**

#### Worst-Case Analysis
- **Method**: Add up all maximum tolerances
- **Formula**: Total tolerance = Σ|individual tolerances|
- **Pros**: 100% of parts guaranteed to work
- **Cons**: Very conservative, expensive (tight tolerances)
- **Use when**: Safety-critical, low production volume, no rework allowed

**Example**:
```
Part A: 10.0 ±0.1 mm
Part B: 20.0 ±0.2 mm  
Part C: 5.0 ±0.05 mm
Worst-case total: 35.0 ±0.35 mm (0.1+0.2+0.05)
```

#### RSS - Root Sum Square Analysis
- **Method**: Statistical approach assuming normal distribution
- **Formula**: Total tolerance = √(Σ tolerance²)
- **Pros**: More realistic, allows looser (cheaper) tolerances
- **Cons**: Not 100% guaranteed (typically 99.73% with 3σ)
- **Use when**: High production volume, some scrap acceptable, capable processes

**Example** (same parts as above):
```
RSS total: 35.0 ±0.23 mm
√(0.1² + 0.2² + 0.05²) = √0.0525 = 0.23
```

**Tolerance reduction: 34%!** This means you can use cheaper manufacturing.

#### Monte Carlo Simulation
- **Method**: Computer simulation of thousands of random assemblies
- **How it works**: Randomly generates dimensions within tolerance, assembles them
- **Pros**: Most realistic, handles any distribution type, gives probability curves
- **Cons**: Requires computer, more complex to understand
- **Use when**: Complex assemblies, mixed distributions, need confidence levels

### 3. **Distribution Types**

Manufacturing processes don't produce parts randomly—they follow patterns:

**Normal (Gaussian) Distribution**:
- Most common in nature and manufacturing
- Bell curve: most parts near nominal, fewer at extremes
- Example: CNC machining, injection molding
- **Use for**: Stable, controlled processes

**Uniform Distribution**:
- All values equally likely within range
- Flat probability across tolerance
- Example: Manual operations, rough processes
- **Use for**: Conservative estimates, poorly controlled processes

**Bernoulli Distribution**:
- Binary: parts are either at min or max tolerance
- Represents worst-case within statistical framework
- Example: Go/no-go gauging effects
- **Use for**: Very tight specifications

**Trapezoidal Distribution**:
- More realistic than uniform, less than normal
- Higher probability near center, but not bell-shaped
- Example: Processes with some control but not optimized
- **Use for**: Semi-controlled processes

### 4. **Clearance Fit Analysis**

When a shaft fits into a hole, three types of fits are possible:

**Clearance Fit**:
- Shaft is always smaller than hole
- Min clearance > 0
- Parts can move relative to each other
- Example: Door hinge, bearing with play

**Transition Fit**:
- Sometimes clearance, sometimes interference
- Min clearance < 0, Max clearance > 0
- Assembly may require light force
- Example: Precision location pins

**Interference Fit**:
- Shaft is always larger than hole
- Max clearance < 0
- Requires force to assemble (press fit)
- Example: Bearing on shaft, permanent assembly

**ISO 286 Standard**:
- International standard for hole/shaft fits
- Hole basis: H6, H7, H8, H9 (hole is fixed)
- Shaft basis: f6, g6, h6, k6, n6 (varies for fit type)
- Lower number = tighter tolerance (H6 tighter than H7)

### 5. **Process Capability Indices**

These tell you if your manufacturing process is good enough:

**Cp (Process Capability)**:
- Formula: Cp = (Tolerance Spec) / (6σ)
- Measures potential capability if perfectly centered
- **Cp ≥ 1.67**: Excellent (capable of Six Sigma quality)
- **Cp ≥ 1.33**: Good (commonly accepted minimum)
- **Cp ≥ 1.0**: Marginal (barely capable)
- **Cp < 1.0**: Poor (incapable, will produce defects)

**Cpk (Process Capability Index)**:
- Accounts for process centering (is the process on target?)
- Formula: Cpk = min[(USL - μ)/(3σ), (μ - LSL)/(3σ)]
- Where USL = upper spec limit, LSL = lower spec limit, μ = process mean
- **Always ≤ Cp** (equals Cp only when perfectly centered)

**Rule of thumb**:
- **Cpk ≥ 1.33**: Use RSS analysis (saves money)
- **Cpk < 1.33**: Use worst-case analysis (avoid defects)

**Expected defect rates**:
- Cpk = 2.0: 0.002 PPM (parts per million)
- Cpk = 1.67: 0.6 PPM
- Cpk = 1.33: 63 PPM
- Cpk = 1.0: 2,700 PPM
- Cpk = 0.67: 45,500 PPM

### 6. **Manufacturing Process Capabilities**

Different processes have different natural capabilities:

| Process | Typical Tolerance | Cpk | Notes |
|---------|------------------|-----|-------|
| CNC Turning | ±0.025 mm | 1.67 | Excellent for tight tolerances |
| CNC Milling | ±0.05 mm | 1.33 | Good precision |
| Injection Molding | ±0.15 mm | 1.0 | Varies with size/material |
| Sheet Metal | ±0.25 mm | 0.83 | Harder to control |
| 3D Printing (FDM) | ±0.5 mm | 0.67 | Lowest precision |
| 3D Printing (SLA) | ±0.15 mm | 1.0 | Better than FDM |

**Key insight**: Don't specify ±0.01 mm tolerance if you're using 3D printing!

---

## 🛠️ How to Use This Tool

### Step-by-Step Guide

1. **Add Your Dimensions**
   - Click "Add Dimension" for each part in your assembly
   - Enter nominal value and tolerance
   - Choose bilateral (±) or unilateral tolerance

2. **Set Direction**
   - **Add (+)**: Dimension increases the total (e.g., part length)
   - **Subtract (−)**: Dimension decreases the total (e.g., gap, clearance)

3. **Choose Distribution**
   - Select based on your manufacturing process
   - When in doubt, use "Normal" for controlled processes
   - Use "Uniform" for conservative estimates

4. **View Results**
   - Go to "Results" tab
   - Compare Worst-Case vs RSS
   - Check process capability (Cp/Cpk)
   - Run Monte Carlo for detailed probability

5. **Fit Analysis** (Optional)
   - Enable if analyzing hole/shaft fit
   - Enter hole and shaft dimensions
   - See clearance/interference results

### Example Workflow

**Scenario**: Designing a battery compartment

1. Add dimensions:
   - Compartment depth: 8.0 mm ±0.1 mm (Add)
   - Battery height: -7.5 mm ±0.1 mm (Subtract)
   - Cover thickness: -0.4 mm ±0.05 mm (Subtract)

2. Results show:
   - Nominal clearance: 0.1 mm
   - Worst-case min: -0.15 mm ❌ (interference!)
   - RSS min: -0.05 mm ⚠️ (still risky)

3. Solution: Increase compartment depth or reduce battery tolerance

---

## 📊 Interpreting Results

### When to Use Which Method

| Situation | Recommended Method | Why |
|-----------|-------------------|-----|
| Safety-critical (aircraft, medical) | Worst-Case | 100% guarantee |
| Low volume production (< 1000) | Worst-Case | Can't afford rejects |
| High volume, capable process | RSS | Cost savings |
| Very high volume (millions) | Monte Carlo | Optimize for cost vs quality |
| Prototype/feasibility | RSS or Monte Carlo | Realistic estimate |
| Mixed processes | Monte Carlo | Different distributions |

### Reading Monte Carlo Results

**Percentiles explained**:
- **P0.1%**: Only 1 in 1000 parts will be smaller
- **P1%**: Only 1 in 100 parts will be smaller
- **P50%**: Median (half above, half below)
- **P99%**: Only 1 in 100 parts will be larger
- **P99.9%**: Only 1 in 1000 parts will be larger

**Example interpretation**:
```
P1% = 9.85 mm
P99% = 10.15 mm
```
Means: 98% of assemblies will be between 9.85 and 10.15 mm.

### Common Mistakes to Avoid

❌ **Using worst-case always**: Costs too much  
✅ Use RSS when Cpk ≥ 1.33

❌ **Assuming normal distribution for all processes**: Not always true  
✅ Match distribution to process

❌ **Ignoring process capability**: Specifying impossible tolerances  
✅ Check if your supplier can achieve it

❌ **Forgetting direction**: Adding when should subtract  
✅ Think carefully about + vs −

❌ **Not validating with prototypes**: Calculations aren't everything  
✅ Build and measure real parts

---

## 🎯 Practical Examples

### Example 1: Simple Linear Stack

**Problem**: Three blocks stacked vertically
- Block A: 10 mm ±0.1 mm
- Block B: 20 mm ±0.2 mm
- Block C: 15 mm ±0.15 mm
- Question: What's the total height range?

**Solution**:
```
Nominal: 10 + 20 + 15 = 45 mm

Worst-case: ±(0.1 + 0.2 + 0.15) = ±0.45 mm
Range: 44.55 to 45.45 mm

RSS: ±√(0.1² + 0.2² + 0.15²) = ±0.27 mm
Range: 44.73 to 45.27 mm
```

### Example 2: Clearance Calculation

**Problem**: Shaft in bearing
- Bearing ID: 10.00 mm +0.10/-0 mm (10.00 to 10.10)
- Shaft OD: 9.95 mm ±0.05 mm (9.90 to 10.00)
- Question: What's the clearance?

**Solution**:
```
Max clearance: 10.10 - 9.90 = 0.20 mm
Min clearance: 10.00 - 10.00 = 0.00 mm
Fit type: Clearance (min ≥ 0)
```

### Example 3: Gap in Assembly

**Problem**: Cover plate assembly
- Box width: 100 mm ±0.5 mm (Add)
- Cover width: -99 mm ±0.3 mm (Subtract)
- Question: What's the gap?

**Solution**:
```
Nominal gap: 100 - 99 = 1.0 mm

Worst-case:
- Max gap: 100.5 - 98.7 = 1.8 mm
- Min gap: 99.5 - 99.3 = 0.2 mm

RSS:
- Total tolerance: √(0.5² + 0.3²) = 0.58 mm
- Range: 0.42 to 1.58 mm
```

---

## 🚀 Quick Deploy to Vercel

### Method 1: Using Vercel CLI (Recommended)

1. **Install Vercel CLI**
   ```bash
   npm install -g vercel
   ```

2. **Navigate to your project folder**
   ```bash
   cd /path/to/your/folder
   ```

3. **Deploy**
   ```bash
   vercel
   ```
   
   Follow the prompts:
   - Set up and deploy? **Y**
   - Which scope? Select your account
   - Link to existing project? **N**
   - Project name? Press Enter (or choose a name)
   - In which directory is your code? **./`**
   - Auto-detected settings: **Y**

4. **Done!** You'll get a URL like `https://your-project.vercel.app`

### Method 2: Using Vercel Dashboard (Easiest)

1. **Go to [vercel.com](https://vercel.com)** and sign up/login

2. **Click "Add New Project"**

3. **Choose one of these options:**

   **Option A: Upload files directly**
   - Click "Upload" tab
   - Drag and drop your `tolerance-stackup.html` file
   - Click "Deploy"
   
   **Option B: From Git repository**
   - Push your files to GitHub/GitLab/Bitbucket
   - Import the repository in Vercel
   - Vercel will auto-deploy

4. **Configure (if needed)**
   - Root Directory: `./`
   - No build command needed (static HTML)
   - No install command needed

5. **Deploy!** Your site will be live at `https://your-project.vercel.app`

### Method 3: GitHub Integration (Best for Updates)

1. **Create a GitHub repository**
   ```bash
   git init
   git add tolerance-stackup.html README.md
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/yourusername/tolerance-analysis.git
   git push -u origin main
   ```

2. **Connect to Vercel**
   - Go to [vercel.com/new](https://vercel.com/new)
   - Import your GitHub repository
   - Click "Deploy"

3. **Automatic deployments**
   - Every push to `main` branch auto-deploys
   - Pull requests get preview URLs

## Project Structure

```
tolerance-analysis/
├── tolerance-stackup.html    # Main application (standalone)
├── README.md                 # This file
└── vercel.json              # Optional configuration
```

## Optional: Vercel Configuration

Create a `vercel.json` file for custom settings:

```json
{
  "cleanUrls": true,
  "trailingSlash": false,
  "rewrites": [
    { "source": "/", "destination": "/tolerance-stackup.html" }
  ]
}
```

This makes your homepage load the tool directly at the root URL.

## Custom Domain (Optional)

1. Go to your project in Vercel Dashboard
2. Click "Settings" → "Domains"
3. Add your custom domain
4. Update DNS records as instructed

## Features

- ✅ Worst-Case tolerance analysis
- ✅ RSS (Root Sum Square) statistical method
- ✅ Monte Carlo simulation with multiple distributions
- ✅ Clearance fit analysis (hole/shaft)
- ✅ Process capability indicators (Cp, Cpk)
- ✅ Save/load configurations
- ✅ Interactive visualizations
- ✅ No build step required - pure HTML/CSS/JS

## Browser Support

- Chrome/Edge: ✅ Full support
- Firefox: ✅ Full support
- Safari: ✅ Full support
- Mobile browsers: ✅ Responsive design

## Local Development

No build process needed! Simply open `tolerance-stackup.html` in your browser.

## Troubleshooting

**Issue: Page shows 404**
- Make sure your HTML file is in the root directory
- Check that the file is named correctly
- Verify vercel.json rewrites if using custom configuration

**Issue: Fonts not loading**
- The page uses Google Fonts (CDN)
- Ensure you have internet connection
- Fonts will fallback to system fonts if CDN fails

**Issue: Need HTTPS**
- Vercel automatically provides HTTPS
- All deployments get SSL certificates

## Updates

To update your deployed site:

**If using CLI:**
```bash
vercel --prod
```

**If using Git:**
```bash
git add .
git commit -m "Update analysis tool"
git push
```
Vercel will auto-deploy!

## Support

For Vercel-specific issues, visit: https://vercel.com/docs

---

## 📖 References and Further Reading

### Standards and Specifications

1. **ISO 286-1:2010** - Geometrical product specifications (GPS) — ISO code system for tolerances on linear sizes — Part 1: Basis of tolerances, deviations and fits
   - The international standard for hole and shaft tolerances
   - Defines tolerance grades IT01 through IT18

2. **ISO 286-2:2010** - Part 2: Tables of standard tolerance classes and limit deviations for holes and shafts
   - Practical tables for common fits (H7/g6, etc.)

3. **ASME Y14.5-2018** - Dimensioning and Tolerancing
   - American standard for GD&T (Geometric Dimensioning and Tolerancing)
   - Essential for understanding tolerance stack-ups in North America

4. **ISO 2768** - General tolerances for linear and angular dimensions
   - Default tolerances when not specified on drawings

### Books

1. **"Mechanical Tolerance Stack-up and Analysis" by Benton Bryan Fischer** (2nd Edition, 2011)
   - Comprehensive guide to tolerance analysis
   - Excellent for beginners and practitioners
   - ISBN: 978-0849370670

2. **"Dimensioning and Tolerancing Handbook" by Paul J. Drake Jr.** (1999)
   - Reference manual for GD&T and tolerance stack-up
   - ISBN: 978-0070181311

3. **"Statistical Tolerancing in Design for Six Sigma" by Forrest W. Breyfogle III**
   - Advanced statistical methods for tolerance analysis
   - Links Six Sigma methodology to tolerance design

4. **"Tolerance Design: A Handbook for Developing Optimal Specifications" by C.M. Creveling** (1997)
   - Taguchi methods applied to tolerance design
   - Optimization techniques
   - ISBN: 978-0201634730

5. **"The GD&T Hierarchy" by Don Day**
   - Modern approach to geometric tolerancing
   - Practical examples and case studies

### Technical Papers and Articles

1. **"Statistical Tolerance Analysis Using a Modification of Taguchi's Method" by K.C. Kapur** (Technometrics, 1988)
   - Foundational paper on statistical tolerance methods

2. **"Tolerance Analysis of 2-D and 3-D Assemblies" by Kenneth W. Chase et al.** (ADCATS Report No. 94-4, 1994)
   - Brigham Young University research
   - Vector loop method for complex assemblies

3. **"A Comprehensive Study of Three-Dimensional Tolerancing Analysis Methods" by Shen Z. et al.** (Journal of Computing and Information Science in Engineering, 2005)
   - Comparison of different tolerance analysis approaches

4. **"Monte Carlo Simulation of Tolerancing Problems in Discrete-Part Manufacturing and Assembly" by A.T. Soderberg and L. Lindkvist** (CIRP Annals, 1999)
   - Detailed Monte Carlo methodology

### Online Resources

1. **NIST Engineering Statistics Handbook**
   - URL: https://www.itl.nist.gov/div898/handbook/
   - Free, comprehensive resource on statistical methods
   - Section 5.5 covers process capability

2. **MIT OpenCourseWare - Precision Engineering**
   - Course materials on tolerance analysis and precision design
   - Free lectures and problem sets

3. **ASQ (American Society for Quality) - Process Capability Resources**
   - URL: https://asq.org/quality-resources/process-capability
   - Tutorials on Cp, Cpk, and process capability

4. **Machinery's Handbook** (31st Edition)
   - Essential reference for machinists and engineers
   - Tables for standard fits and tolerances
   - Available in print and digital

### Software and Tools

1. **3DCS Variation Analyst** (Commercial)
   - Industry-standard 3D tolerance analysis software
   - Integrates with CAD systems

2. **Cetol 6σ by Sigmetrix** (Commercial)
   - Tolerance analysis within CAD environment
   - Monte Carlo and worst-case analysis

3. **VisVSA (Variation Systems Analysis)** (Commercial)
   - 3D tolerance stack-up analysis
   - Root cause analysis tools

4. **EZtol** (Commercial)
   - 1D tolerance stack-up analysis
   - Educational pricing available

5. **OpenTOL** (Open Source)
   - Free tolerance analysis software
   - Limited to 1D and 2D analysis

### Industry Guidelines

1. **SAE J1100** - Motor Vehicle Dimensions
   - Automotive industry tolerance practices

2. **IPC-2221** - Generic Standard on Printed Board Design
   - Tolerances for PCB manufacturing

3. **Semiconductor Industry Association (SIA) Roadmap**
   - Tolerance requirements for semiconductor manufacturing

### Video Tutorials and Courses

1. **YouTube: "GD&T Basics" by Tec-Ease**
   - Free tutorial series on geometric tolerancing

2. **Coursera: "Manufacturing Process Control"**
   - Statistical process control and capability analysis

3. **LinkedIn Learning: "Tolerance Stack-Up Analysis"**
   - Practical examples and software tutorials

### Professional Organizations

1. **ASME (American Society of Mechanical Engineers)**
   - URL: https://www.asme.org
   - Standards development and training

2. **ASQ (American Society for Quality)**
   - URL: https://asq.org
   - Quality engineering certifications and resources

3. **SME (Society of Manufacturing Engineers)**
   - URL: https://www.sme.org
   - Manufacturing technology and education

### Statistical Background

1. **"Introduction to Statistical Quality Control" by Douglas C. Montgomery** (8th Edition)
   - Comprehensive coverage of SPC and process capability
   - ISBN: 978-1119399308

2. **"Statistics for Engineers and Scientists" by William Navidi**
   - Statistical methods applicable to tolerance analysis
   - Clear explanations of distributions and confidence intervals

3. **"Probability and Statistics for Engineering and the Sciences" by Jay L. Devore**
   - Foundation for understanding Monte Carlo methods
   - ISBN: 978-1305251809

### Case Studies and Applications

1. **Boeing Statistical Tolerancing Handbook**
   - Aerospace industry best practices (internal document)
   - Often referenced in academic papers

2. **Ford Motor Company "Geometric Dimensioning and Tolerancing" Training Manual**
   - Automotive industry approaches

3. **NASA Technical Standards Program - Dimensional Management**
   - Space systems tolerance requirements

### Calculators and Quick References

1. **Tolerance Analysis Calculator** by Engineers Edge
   - URL: https://www.engineersedge.com
   - Free online calculators

2. **Fits and Tolerances Calculator** by ISO Limits & Fits
   - Quick reference for ISO 286 fits

3. **Process Capability Calculator** (Multiple sources available)
   - Calculate Cp, Cpk from measurement data

### Academic Research Centers

1. **Brigham Young University - ADCATS Laboratory**
   - Leading research in tolerance analysis
   - Developed assembly variation analysis methods

2. **MIT - Mechanical Engineering Department**
   - Precision engineering research
   - Tolerancing and metrology

3. **Penn State - Applied Research Laboratory**
   - Manufacturing tolerancing research

---

## 📝 Glossary of Terms

- **Bilateral Tolerance**: Tolerance that varies equally in both directions from nominal (±)
- **Clearance**: Space between mating parts
- **Cp**: Process Capability - ratio of tolerance to process spread
- **Cpk**: Process Capability Index - accounts for process centering
- **GD&T**: Geometric Dimensioning and Tolerancing
- **Interference**: Negative clearance (shaft larger than hole)
- **ISO 286**: International standard for limits and fits
- **LSL**: Lower Specification Limit
- **Monte Carlo**: Simulation method using random sampling
- **Nominal**: Target or ideal dimension
- **Normal Distribution**: Bell curve, most common statistical distribution
- **PPM**: Parts Per Million (defect rate)
- **RSS**: Root Sum Square - statistical tolerance method
- **Stack-Up**: Accumulation of tolerances in an assembly
- **Transition Fit**: Fit that may be clearance or interference
- **Unilateral Tolerance**: Tolerance in only one direction
- **USL**: Upper Specification Limit
- **Worst-Case**: Conservative analysis method adding all tolerances

---

## 🙏 Acknowledgments

This tool is built on decades of research and industrial practice in tolerance analysis. Special recognition to:

- Dr. Kenneth W. Chase (Brigham Young University) for pioneering tolerance analysis research
- The ASME Y14.5 committee for standardizing GD&T practices
- ISO TC 213 for international standards on tolerancing
- The quality engineering community for developing process capability metrics

---

## 📄 License

MIT License - Free to use for educational and commercial purposes

---

## ✨ Contributing

Found an issue or want to improve the tool? Contributions welcome!

- Report bugs or suggest features via issues
- Submit pull requests for improvements
- Share your use cases and examples

---

Built with vanilla HTML, CSS, and JavaScript - no framework required!

**Disclaimer**: This tool is for educational and reference purposes. Always validate critical tolerance analyses with professional engineering software and physical prototypes. Consult with qualified engineers for safety-critical applications.
