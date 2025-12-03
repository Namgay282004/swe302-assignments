# Task 3.2 Active Scan Commands - TAKE SCREENSHOTS

## Task 3.2: Active Vulnerability Analysis

### Frontend Active Scan
```bash
cd /home/namgaywangchuk/Desktop/Fifth-Semester/swe302_assignments/Assignment2

# Active baseline scan with alpha rules for frontend
echo "=== TASK 3.2: FRONTEND ACTIVE SCAN ==="
docker run -v $(pwd):/zap/wrk/:rw -t zaproxy/zap-stable zap-baseline.py \
  -t http://10.34.90.196:4100 \
  -J zap-active-frontend.json \
  -r zap-active-frontend.html \
  -a -d -m 2
```

### Backend API Active Scan  
```bash
# Active baseline scan for backend API
echo "=== TASK 3.2: BACKEND API ACTIVE SCAN ==="
docker run -v $(pwd):/zap/wrk/:rw -t zaproxy/zap-stable zap-baseline.py \
  -t http://10.34.90.196:8081 \
  -J zap-active-backend.json \
  -r zap-active-backend.html \
  -a -d -m 2
```

## Task 3.3: API Security Assessment

### Get JWT Token for API Testing
```bash
# Get authentication token
TOKEN=$(curl -s -X POST http://localhost:8081/api/users/login \
  -H "Content-Type: application/json" \
  -d '{"user":{"email":"security-test@example.com","password":"SecurePass123!"}}' | \
  jq -r '.user.token')

echo "JWT Token obtained: $TOKEN"
```

### Test API Endpoints
```bash
# Test authenticated endpoints
echo "=== TESTING AUTHENTICATED API ENDPOINTS ==="
curl -H "Authorization: Token $TOKEN" http://localhost:8081/api/user
echo ""
curl -H "Authorization: Token $TOKEN" http://localhost:8081/api/articles/
echo ""
curl -H "Authorization: Token $TOKEN" http://localhost:8081/api/tags/
```

### API-Specific Security Scan
```bash
# API endpoint security scan
echo "=== TASK 3.3: API SECURITY SCAN ==="
docker run -v $(pwd):/zap/wrk/:rw -t zaproxy/zap-stable zap-baseline.py \
  -t http://10.34.90.196:8081/api \
  -J zap-api-security.json \
  -r zap-api-security.html \
  -a -d -m 2
```

## Task 3.4: View All Results

### List Generated Reports
```bash
echo "=== ALL GENERATED ZAP REPORTS ==="
ls -la *zap*.html *zap*.json
```

### View Report Statistics
```bash
echo "=== SCAN RESULTS SUMMARY ==="
echo "Active Frontend Scan Results:"
grep -i "FAIL\|WARN\|PASS" zap-active-frontend.html | head -3
echo ""
echo "Active Backend Scan Results:" 
grep -i "FAIL\|WARN\|PASS" zap-active-backend.html | head -3
echo ""
echo "API Security Scan Results:"
grep -i "FAIL\|WARN\|PASS" zap-api-security.html | head -3
```

## Screenshot Checklist:

**Task 3.2 Screenshots:**
- [ ] Frontend active scan execution terminal
- [ ] Backend API active scan execution terminal  
- [ ] Generated active scan HTML reports in browser
- [ ] File listing showing new report files

**Task 3.3 Screenshots:**
- [ ] JWT token generation command
- [ ] Authenticated API endpoint testing
- [ ] API security scan execution terminal
- [ ] API security report in browser

**Task 3.4 Screenshots:**
- [ ] All reports file listing
- [ ] Summary statistics terminal output
- [ ] Final vulnerability counts

## Execute these commands in order and take screenshots at each step!
